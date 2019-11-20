<template lang="pug">
.modal
  .modal-block.card.notification.wrapper(@click="stopPropagation")

    form.denial-reason(v-if="linkStep1")
      span Добавление почты
      ErrorsBlock(:errors="errors")
      label.form_line
        input(type="text", id="linkEmail", placeholder="Введите ваш email")
      label.form_line
        input(type="password", name="linkPassword", id="linkPassword", placeholder="Придумайте пароль", @focus="setActiveLine")
        button.btn(type="button", @click="changePasswordVisibility") Показать пароль
          svg.show(xmlns="http://www.w3.org/2000/svg", viewBox="0 0 511.998 511.998")
            circle(cx="257.013", cy="256.003", r="38.999")
            path(d="M257.011,142.002c-62.858,0-113.997,51.139-113.997,113.997s51.139,113.997,113.997,113.997s113.997-51.139,113.997-113.997S319.869,142.002,257.011,142.002z M257.011,329.997c-40.803,0-73.998-33.195-73.998-73.998c0-40.803,33.195-73.998,73.998-73.998s73.998,33.195,73.998,73.998C331.009,296.802,297.814,329.997,257.011,329.997z")
            path(d="M499.046,212.345c-10.618-16.306-40.581-58.167-89.352-94.633c-49.438-36.965-100.997-55.709-153.246-55.7c-52.175,0-103.779,18.693-153.379,55.558c-48.988,36.41-79.191,78.219-89.903,94.504c-17.555,26.689-17.555,61.178,0,87.8c10.712,16.286,40.915,58.095,89.903,94.505c49.6,36.865,101.203,55.558,153.379,55.558c80.325,0,144.917-44.788,184.957-82.359c8.055-7.558,8.458-20.214,0.9-28.269c-7.559-8.056-20.214-8.458-28.269-0.9c-34.777,32.632-90.347,71.53-157.59,71.53c-97.104,0-177.606-83.001-209.865-132.045c-8.772-13.336-8.772-30.568,0-43.904c32.259-49.044,112.761-132.044,209.865-132.044c97.101,0,177.112,83.08,209.081,132.17c8.635,13.259,8.635,30.393,0,43.652c-6.027,9.256-3.41,21.645,5.846,27.673c9.255,6.027,21.646,3.411,27.673-5.846C516.315,273.134,516.315,238.863,499.046,212.345z")
          svg.hide(xmlns="http://www.w3.org/2000/svg", viewBox="0 0 511.998 511.998")
            path(d="M506.14,477.85L400.745,372.453c-0.161-0.167-0.324-0.33-0.49-0.49L140.272,111.98c-0.176-0.183-0.354-0.361-0.536-0.536L34.158,5.866c-7.811-7.811-20.473-7.811-28.283,0c-7.81,7.811-7.811,20.473,0,28.283l89.388,89.388c-45.563,35.873-73.81,75.931-82.097,88.53c-17.554,26.688-17.554,61.178,0,87.866c10.712,16.285,40.915,58.094,89.902,94.504c49.599,36.865,101.203,55.557,153.378,55.557c43.005,0,85.726-12.796,127.205-38.068l94.208,94.208c3.905,3.905,9.023,5.858,14.141,5.858s10.237-1.953,14.141-5.858C513.951,498.322,513.951,485.66,506.14,477.85zM191.476,219.749l27.519,27.519c-2.892,12.647,0.587,26.456,10.438,36.307c9.851,9.851,23.659,13.329,36.307,10.438l26.529,26.529c-10.947,6.172-23.35,9.455-36.26,9.455c-40.803,0-73.997-33.195-73.997-73.997C182.011,243.139,185.303,230.714,191.476,219.749z M256.445,409.993c-97.103,0-177.604-82.999-209.863-132.043c-8.772-13.335-8.772-30.568,0-43.903c7.621-11.586,34.189-49.233,77.183-82.007l38.732,38.732c-13.303,19.018-20.485,41.649-20.485,65.227c0,62.858,51.138,113.996,113.996,113.996c23.673,0,46.244-7.168,65.237-20.476l33.167,33.167C321.998,400.821,289.136,409.993,256.445,409.993z")
            path(d="M499.356,212.844c-0.103-0.167-0.208-0.334-0.315-0.5c-10.619-16.305-40.582-58.166-89.351-94.632c-49.438-36.965-100.997-55.708-153.245-55.708c-19.887,0-40.046,2.811-59.915,8.353c-10.64,2.969-16.858,14-13.891,24.638c2.968,10.639,13.995,16.856,24.638,13.891c16.37-4.567,32.913-6.882,49.167-6.882c96.691,0,176.434,82.377,208.671,131.543c0.103,0.167,0.209,0.334,0.318,0.499c4.296,6.532,6.567,14.122,6.567,21.952c0,7.83-2.271,15.42-6.567,21.952c-9.62,14.625-22.511,30.668-36.301,45.174c-7.61,8.006-7.29,20.664,0.716,28.274c3.87,3.679,8.827,5.504,13.776,5.504c5.288,0,10.568-2.085,14.499-6.221c19.709-20.733,33.027-39.044,40.728-50.752c8.602-13.078,13.148-28.269,13.148-43.933C511.998,240.644,507.63,225.744,499.356,212.844z")
            path(d="M367.352,231.459c-9.4-42.81-43.082-76.796-85.812-86.582c-10.766-2.461-21.493,4.264-23.959,15.03c-2.465,10.767,4.264,21.493,15.03,23.959c27.722,6.348,49.574,28.396,55.672,56.171c2.051,9.344,10.327,15.714,19.515,15.714c1.42,0,2.863-0.152,4.308-0.47C362.895,252.913,369.72,242.247,367.352,231.459z")
      button.btn.btn-default.container.form-submit(type="submit", id="sendButton" @click.prevent="sendEmail")
        span Отправить
      button.btn.btn-default.btn-reverse(type="button", @click="$emit('closeModal')") Отмена

    form.denial-reason(v-if="linkStep2")
      span Подтверждение кода
      p.hint Для смены пароля введите код, который мы отправили вам на почту
      ErrorsBlock(:errors="errors")
      label.form_line
        input(type="text", id="linkCode", placeholder="Введите код с письма")
      button.btn.btn-default.container.form-submit(type="submit", id="sendCode" @click.prevent="sendCode")
        span Отправить
      button.btn.btn-default.btn-inverted(type="submit" @click.prevent="resendCode" :disabled="resend_disabled") {{ resend_disabled ? `ОТПРАВИТЬ ПОВТОРНО ЧЕРЕЗ ${this.resend_time} с.` : `ОТПРАВИТЬ ПОВТОРНО` }}
      button.btn.btn-default.btn-reverse(type="button", @click="$emit('closeModal')") Отмена

</template>

<script>
import ErrorsBlock from '../ErrorsBlock.vue'

export default {
    name: "ModalLinkEmail",
    components : {
        ErrorsBlock
    },
    data() {
        return {
            resend_time: 45,
            resend_disabled : true,
            interval : 0, // таймер отсчёта

            linkStep1: true,
            linkStep2: false,
            errors : [],
            token: {}
        }
    },
    mounted: function () {
        let self = this;
        this.startTimer();
    },
    methods : {
        closeModal: function() {
            this.$emit('closeModal');
        },
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
        // переотправить код на почту
        resendCode : function ($event) {
            let self = this;

            let button = $event.target;

            if (button.classList.contains('loading')) return;

            self.showErrors([]);

            button.classList.add('loading');

            axios({
                method : 'POST',
                url : '/api/settings/email/resend',
                data : self.token
            })
            .then(response => {
                // код переотправлен
                button.classList.remove('loading');
                self.startTimer();
            })
            .catch(res => {
                button.classList.remove('loading');

                self.showErrors(res.response.data.message);
            });

        },
        setActiveLine: function ($event) {
          let formLines = document.querySelectorAll('.form-login_line')
          for (let l = 0; l < formLines.length; l++) {
            formLines[l].classList.remove('active')
          }
          $event.target.parentElement.classList.add('active')
        },
        changePasswordVisibility: function ($event) {
          let button = $event.target
          if (button.tagName !== 'BUTTON') {
            button = button.parentElement
          }
          if (button.tagName !== 'BUTTON') {
            button = button.parentElement
          }
          button.classList.toggle('active')
          if (button.previousSibling.getAttribute('type') === 'password') {
            button.previousSibling.setAttribute('type', 'text')
          } else {
            button.previousSibling.setAttribute('type', 'password')
          }
        },
        stopPropagation: function (e) {
            e.stopPropagation();
        },
        sendCode: function ($event) {
            let self = this;
            let button = $event.target;

            if (button.classList.contains('loading')) return;

            self.showErrors([]);

            let code = document.getElementById('linkCode').value.trim();

            if (!code.length) {
                return self.showErrors([
                    "Введите код"
                ]);
            }

            button.classList.add('loading');
            button.innerHTML = '&nbsp;';

            axios({
                method : 'POST',
                url : '/api/settings/email/confirm',
                data : {
                    id : self.token.id,
                    code: code
                }
            })
            .then(response => {

                document.location.reload();

            })
            .catch(res => {
                self.showErrors(res.response.data.message);
                button.classList.remove('loading');
                button.innerHTML = 'Отправить';
            });

        },
        sendEmail: function ($event) {
            let self = this;
            let button = $event.target;

            if (button.classList.contains('loading')) return;

            self.showErrors([]);

            let email = document.getElementById('linkEmail').value;
            let password = document.getElementById('linkPassword').value;

            if (!email.length) {
                return self.showErrors([
                    "Введите ваш email"
                ]);
            }

            let match = email.match(this.$store.state.email_regex);

            if (!match || match[0].length !== email.length) {
                return self.showErrors([
                    "Введите валидный email"
                ]);
            }

            if (!password.length) {
                return self.showErrors([
                    "Введите ваш пароль"
                ]);
            }

            if (password.length > 32 || password.length < 6) {
                return self.showErrors([
                    "Пароль должен состоять минимум из 6 символов",
                    "Пароль должен состоять максимум из 32 символов"
                ]);
            }

            match = password.match(this.$store.state.password_regex);

            if (!match || match[0].length !== password.length) {
                return self.showErrors([
                    "Пароль должен содержать только буквы латинского алфавита и цифры"
                ]);
            }

            button.classList.add('loading');
            button.innerHTML = '&nbsp;';

            axios({
                method : 'POST',
                url : '/api/settings/email/link',
                data : {
                    email : email,
                    password: password
                }
            })
            .then(response => {

                self.token = response.data.token;
                self.linkStep1 = false;
                self.linkStep2 = true;
                self.startTimer();

            })
            .catch(res => {
                self.showErrors(res.response.data.message);
                button.classList.remove('loading');
                button.innerHTML = 'Отправить';
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

<style scoped lang="sass">
@import "../../../sass/settings.scss"
.modal
  position: fixed
  z-index: 1500
  top: 0
  left: 0
  right: 0
  bottom: 0
  overflow: hidden
  display: flex
  justify-content: center
  align-items: center
  padding: 10px
  background: rgba(58,72,89,.6)
  &-block
    width: 100%
    height: auto
    margin: auto 0
    border: none
    border-radius: 3px
    background: $white
    box-shadow: 0 4px 12px 0 rgba(34, 40, 46, .1)
    padding: 16px 12px
    transition: transform 0.3s ease-in-out
  &.skew-enter, &.skew-leave-to
    transform: scale(1)
  &.skew-leave, &.skew-enter-to
    transform: scale(1)
  &.skew-enter .modal-block, &.skew-leave-to .modal-block
      transform: scale(0)
  &.skew-leave .modal-block, &.skew-enter-to .modal-block
      transform: scale(1)

.denial-reason
  display: block
  width: 100%
  #linkPassword, #linkPasswordRepeat
    background-image: url('/images/icons/168-padlock.svg')
    background-repeat: no-repeat
    background-position: 11px 9px!important
    background-size: 13px auto
    + .btn
      font-size: 0
      line-height: 0
      color: transparent
      position: absolute
      width: 31px
      height: 31px
      right: 0
      top: 50%
      margin-top: -15.5px
      svg
        position: absolute
        left: 9px
        top: 9px
        width: 13px
        height: auto
        transition: opacity 0.15s ease-in-out
        border: 0
        path, circle
          fill: $grey
      svg.show
        opacity: 1
      svg.hide
        opacity: 0
      &.active
        svg.show
          opacity: 0
        svg.hide
          opacity: 1
    &:hover, &:focus
      background-image: url('/images/icons/168-padlock-dark.svg')
      + .btn svg
        path, circle
          fill: $dark

  #linkEmail
    background-image: url('/images/icons/159-email.svg')
    background-repeat: no-repeat
    background-position: 11px 9px!important
    background-size: 13px auto
    &:hover, &:focus
      background-image: url('/images/icons/159-email-dark.svg')

  #sendButton
    transition: background 0.3s ease-in-out, color 0.3s ease-in-out, background-position 0s
  #sendButton.loading
    background-image: url('/images/loading-button.svg')
    background-repeat: no-repeat
    background-size: contain
    background-position: center
  .form_line
    position: relative
    margin-bottom: 12px
    display: block
    width: 100%
  span
    text-align: center
    display: block
    font-size: $text-l-medium
    font-weight: 500
    margin-bottom: 12px
  select
    display: block
    width: 100%
    outline: none
    background: $grey-lighter
    padding: 8px 16px
    color: $dark
    font-weight: 500
    font-family: $arial
    transition: border-color 0.3s ease-in-out, background-image 0.3s ease-in-out
    -webkit-appearance: none
    &::placeholder
      color: $grey
      font-weight: 500
    &:hover, &:focus
      border-color: $bright
  input
    padding: 8px 36px
  .form-submit
    margin-bottom: 12px
    display: flex
    font-size: $text-small
    justify-content: center
    font-weight: 700
    span
      display: inline-block
      margin-right: 3px
      margin-bottom: 0
      font-size: inherit
      font-weight: inherit
    svg
      height: 12px
      width: auto
      margin-left: 3px
      path
        fill: $white
    &:hover, &:focus
      svg path
        fill: $bright
.btn-reverse
  font-size: $text-small
  font-weight: 700

.btn-inverted
    border: 1px solid
    background: white
    color: $grey
    font-size: 11px
    margin-bottom: 12px
    &:hover, &:focus
        color: $bright
.hint
  font-weight: 500
  font-size: $text-medium
  margin: 0 10px 20px
</style>
