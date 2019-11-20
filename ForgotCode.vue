<template lang='pug'>
.login_form
  h1.p_title Восстановление пароля
  ErrorsBlock(:errors="errors")
  p.hint Для смены пароля введите код, который мы отправили вам на почту
  form.form-login
    label.form-login_line
      span.u-visually-hidden Введите код
      input(type="text", name="loginCode", id="loginCode", placeholder="Введите код", @focus="setActiveLine")

    button.btn.btn-default(type="submit" id="loginButton" @click.prevent="sendCode") ПОДТВЕРДИТЬ
    button.btn.btn-default.btn-inverted(type="submit" @click.prevent="resendCode" :disabled="resend_disabled") {{ resend_disabled ? `ОТПРАВИТЬ ПОВТОРНО ЧЕРЕЗ ${this.resend_time} с.` : `ОТПРАВИТЬ ПОВТОРНО` }}
</template>

<script>
import ErrorsBlock from './ErrorsBlock.vue'

export default {
  name: 'ForgotCode',
  components : {
      ErrorsBlock
  },
  data() {
      return {
          resend_time: 45,
          resend_disabled : true,
          interval : 0, // таймер отсчёта
          change_token : false,
          errors : []
      }
  },
  mounted: function () {
      let self = this;
      this.startTimer();
  },
  computed: {
      backTimer() {
          return this.resend_disabled ? `ОТПРАВИТЬ ПОВТОРНО ЧЕРЕЗ ${this.resend_time} с.` : `ОТПРАВИТЬ ПОВТОРНО`
      }
  },
  methods: {
    startTimer: function () {
        clearInterval(this.interval);

        this.resend_time = 45;
        this.resend_disabled = true;

        let self = this;

        this.interval = setInterval(function() {

            let time = self.resend_time - 1;

            self.resend_time = (time > 0) ? time : 0;
            self.resend_disabled = (time > 0) ? true : false;

        }, 1000);
    },
    setActiveLine: function ($event) {
      let formLines = document.querySelectorAll('.form-login_line')
      for (let l = 0; l < formLines.length; l++) {
        formLines[l].classList.remove('active')
      }
      $event.target.parentElement.classList.add('active')
    },
    // отправить код на проверку
    sendCode : function ($event) {
        let self = this;
        let button = $event.target;

        // обнуляем ошибки
        self.showErrors([]);
        
        let code = document.getElementById('loginCode').value;

        if (!code.length) {
            return self.showErrors([
                "Введите код"
            ]);
        }

        if (button.classList.contains('loading')) return;

        button.classList.add('loading');
        button.innerHTML = '&nbsp;';

        // токен из Auth.vue
        let forgot_token = this.$parent.forgot_token;


        axios({
            method : 'POST',
            url : '/forgot/confirm',
            data : {
                id : forgot_token.id,
                code : code
            }
        })
        .then(response => {
            let change_token = Object.assign(forgot_token, { code : code });
            self.$emit('openChangePasswordForm', change_token);
        })
        .catch(res => {
            self.showErrors(res.response.data.message);
            button.classList.remove('loading');
            button.innerHTML = 'ПОДТВЕРДИТЬ';
        });

    },
    // переотправить код на почту
    resendCode : function ($event) {
        let self = this;

        let button = $event.target;

        if (button.classList.contains('loading')) return;

        button.classList.add('loading');

        // токен из Auth.vue
        let forgot_token = this.$parent.forgot_token;

        axios({
            method : 'POST',
            url : '/forgot/resend',
            data : forgot_token
        })
        .then(response => {
            // код переотправлен
            button.classList.remove('loading');
            self.startTimer();
        })
        .catch(res => {
            button.classList.remove('loading');
        });

    },
    showErrors : function (errors) {
        if (typeof errors === 'string') {
            errors = [errors];
        }

        this.errors = errors;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="sass">
@import "../../sass/settings.scss"
.login_form
  width: 100%
  height: 100%
  max-width: 456px
  margin: 0 auto
  display: flex
  flex-direction: column
  justify-content: center
  align-items: center
  text-align: center
  color: $grey
  .p_title
    font-weight: 700
    font-size: $text-large
    margin: 0 0 12px
    color: $dark
  .hint
    font-weight: 500
    font-size: $text-medium
    margin: 0 10px 20px
  .link
    font-weight: 500
    color: $grey
  .form-login
    width: 100%
    margin-bottom: 16px
    &_line
      width: 100%
      display: block
      margin-bottom: 10px
      position: relative
    #loginButton
      transition: background 0.3s ease-in-out, color 0.3s ease-in-out, background-position 0s
    #loginButton.loading
      background-image: url('/images/loading-button.svg')
      background-repeat: no-repeat
      background-size: contain
      background-position: center
    #loginCode
      background-image: url('../../images/icons/086-qr-code.svg')
      background-repeat: no-repeat
      background-position: 11px 9px!important
      background-size: 13px auto
      &:hover, &:focus
        background-image: url('../../images/icons/086-qr-code-dark.svg')
    .btn-default
      text-transform: uppercase
      font-weight: 700
      font-size: $text-medium
      &.btn-inverted
        border: 1px solid
        background: white
        color: $grey
        margin-top: 10px
        &:hover, &:focus
          color: $bright
</style>
