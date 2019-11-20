<template lang='pug'>
.login_form
  h1.p_title Регистрация
  ErrorsBlock(:errors="errors")
  p.hint Для завершения регистрации введите код, который мы отправили вам на почту
  form.form-login
    label.form-login_line
      span.u-visually-hidden Введите код
      input(type="text", name="loginCode", id="loginCode", placeholder="Введите код", @focus="setActiveLine")

    button.btn.btn-default(type="submit" id="loginButton" @click.prevent="sendCode") ЗАРЕГИСТРИРОВАТЬСЯ
    button.btn.btn-default.btn-inverted(type="submit" @click.prevent="resendCode" :disabled="resend_disabled") {{ resend_disabled ? `ОТПРАВИТЬ ПОВТОРНО ЧЕРЕЗ ${this.resend_time} с.` : `ОТПРАВИТЬ ПОВТОРНО` }}
</template>

<script>
import ErrorsBlock from './ErrorsBlock.vue'

export default {
  name: 'RegistrationCode',
  components : {
      ErrorsBlock
  },
  data() {
      return {
          resend_time: 45,
          resend_disabled : true,
          interval : 0, // таймер отсчёта
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

        if (button.classList.contains('loading')) return;

        // обнуляем ошибки
        self.showErrors([]);

        let code = document.getElementById('loginCode').value;

        if (!code.length) {
            return self.showErrors([
                "Введите код"
            ]);
        }

        button.classList.add('loading');
        button.innerHTML = '&nbsp;';

        // токен из Auth.vue
        let resend_token = this.$parent.resend_token;

        axios({
            method : 'POST',
            url : '/signup/confirm',
            data : {
                id : resend_token.id,
                code : document.getElementById('loginCode').value
            }
        })
        .then(response => {
            window.location = "/#success=registration_done";
        })
        .catch(res => {
            self.showErrors(res.response.data.message);
            button.classList.remove('loading');
            button.innerHTML = 'ЗАРЕГИСТРИРОВАТЬСЯ';
        });

    },
    // переотправить код на почту
    resendCode : function ($event) {
        let self = this;

        let button = $event.target;

        if (button.classList.contains('loading')) return;

        button.classList.add('loading');

        // токен из Auth.vue
        let resend_token = this.$parent.resend_token;

        axios({
            method : 'POST',
            url : '/signup/resend',
            data : resend_token
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

@media screen and (min-width: 920px)
  .login_form
    margin-top: auto
    margin-bottom: auto
    height: auto
    background: $white
    border-radius: 8px
    padding: 40px 30px 35px
    max-width: 540px
    .logo
      display: none
    .p_title
      font-size: 48px
      margin-bottom: 20px
    .hint
      margin-bottom: 40px
    .form-login
      margin-bottom: 32px
      &_line
        margin-bottom: 20px
      input
        border-radius: 6px
        padding: 18px 70px 16px
        font-size: 1em
        border-width: 3px
        &#loginCode
          background-size: 26px 26px
          background-position: 23px center !important
      .btn-default
        border-radius: 6px
        padding: 19px 20px 16px
        border-width: 3px
        margin-top: 44px
        font-size: 1em
        font-weight: 700
        &.btn-inverted
          margin-top: 20px
</style>
