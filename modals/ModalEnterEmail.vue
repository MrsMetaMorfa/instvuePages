<template lang="pug">
  .modal(@click="$emit('closeModal')")
    .modal-block.card.notification.wrapper(@click="stopPropagation")
      form.denial-reason
        label.form_line
          span Восстановление пароля
          ErrorsBlock(:errors="errors")
          input(type="text",
                id="forgotEmail",
                placeholder="Введите ваш email")
        button.btn.btn-default.container.form-submit(type="submit", id="sendButton" @click.prevent="sendEmail")
          span Отправить
        button.btn.btn-default.btn-reverse(type="button", @click="$emit('closeModal')") Отмена

</template>

<script>
import ErrorsBlock from '../ErrorsBlock.vue'

export default {
    name: "ModalEnterEmail",
    components : {
        ErrorsBlock
    },
    data() {
        return {
            errors : []
        }
    },
    methods : {
        stopPropagation: function (e) {
            e.stopPropagation();
        },
        sendEmail: function ($event) {
            let self = this;
            let button = $event.target;

            if (button.classList.contains('loading')) return;

            self.showErrors([]);

            let email = document.getElementById('forgotEmail').value;

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

            button.classList.add('loading');
            button.innerHTML = '&nbsp;';

            axios({
                method : 'POST',
                url : '/forgot/create',
                data : {
                    email : email
                }
            })
            .then(response => {
                self.$emit('toggleForgotCodeForm', response.data.token)
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
  #sendButton
    transition: background 0.3s ease-in-out, color 0.3s ease-in-out, background-position 0s
  #sendButton.loading
    background-image: url('/images/loading-button.svg')
    background-repeat: no-repeat
    background-size: contain
    background-position: center
  .form_line
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
    padding: 8px 16px
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
</style>
