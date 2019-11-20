<template lang="pug">
  .modal(@click="closeModal(null)" id="step3modal")
    .modal-block.card.notification.wrapper(@click="stopPropagation")
      .tutorial_helper(v-if="showTutorialHelper")
        p <span>Оставьте причину</span> и <span>рекомендации</span> для пользователя, почему вы не подписались.<br><span>Получите</span> дополнительную <span>энергию</span> и <span>помогите пользователю</span> сделать его аккаунт еще лучше
      form.denial-reason
        label.form_line
          span Почему вас не заинтересовал этот профиль?
          ErrorsBlock(:errors="errors")
          select(name="rotationDenialReason",
                id="rotationDenialReason",
                required=true)
            option(disabled=true, selected=true, value=0) Укажите причину...
            option(v-for='reason in this.$store.state.sub_denial_reasons',
                  :value='reason.id',
                  :id='reason.id') {{ reason.value }}
        label.form_line
          span.u-visually-hidden Оставьте свою рекомендацию
          input(type="text",
                maxlength="256",
                name="rotationRecommendation",
                id="rotationRecommendation",
                placeholder="Оставьте свою рекомендацию")
        button.btn.btn-default.container.form-submit(@click.prevent="closeModal({decision:2})")
          span Отправить
          | +
          svg(xmlns="http://www.w3.org/2000/svg", viewBox="0 0 512 512")
            path(d="M435.243,193.711c-7.943-17.294-24.785-28.037-43.952-28.037H323.08l80.803-135.489c3.68-6.168,3.761-13.837,0.214-20.081C400.549,3.858,393.921,0,386.739,0H197.113c-7.519,0-14.402,4.226-17.802,10.933L79.966,206.826c-11.117,21.828-10.111,47.329,2.69,68.214c12.803,20.885,35.069,33.353,59.565,33.353h69.865l-69.475,142.542c-9.287,18.852-3.868,40.604,13.176,52.896c7.592,5.475,16.234,8.17,24.809,8.169c10.676,0,21.247-4.178,29.542-12.365c0.362-0.358,0.712-0.73,1.045-1.114l154.695-178.034c7.231-8.322,6.346-20.929-1.975-28.16c-8.324-7.232-20.931-6.347-28.16,1.975L181.667,471.625c-0.598,0.508-1.26,0.742-2.528-0.173c-1.573-1.135-1.176-1.941-0.679-2.95l83.504-171.325c3.014-6.186,2.63-13.49-1.017-19.325c-3.648-5.836-10.044-9.381-16.927-9.381h-101.8c-10.655,0-19.959-5.21-25.527-14.295c-5.568-9.084-5.989-19.74-1.139-29.265l93.815-184.99h142.223L270.79,175.411c-3.68,6.168-3.761,13.837-0.214,20.081s10.175,10.103,17.357,10.103h103.357c4.769,0,6.856,2.994,7.676,4.779c0.839,1.826,1.771,5.447-1.419,9.162c-7.183,8.364-6.226,20.966,2.137,28.149c8.365,7.182,20.967,6.227,28.148-2.137C440.379,230.937,443.219,211.074,435.243,193.711z")
        button.btn.btn-default.btn-reverse(type="button", @click.prevent="closeModal({decision:3})") Пропустить

</template>

<script>
import ErrorsBlock from '../ErrorsBlock.vue'

export default {
    name: "ModalBasic",
    components: {
        ErrorsBlock
    },
    data() {
        return {
            errors : []
        }
    },
    computed: {
      showTutorialHelper: function() {
        return !this.$store.state.tutorial.done
          && this.$store.state.tutorial.step === 4;
      }
    },
    methods: {
        closeModal: function (result) {

            if (!result) {
                // клик мимо модалки
                return this.$emit('closeModal', false);
            }

            this.showErrors([]);

            if (result.decision === 2) {
                // если не подписался, то должна быть причина
                if (!Number(document.getElementById('rotationDenialReason').value)) {
                    return this.showErrors("Выберите причину, по которой вы не подписались")
                }
            }

            result.reason = Number(document.getElementById('rotationDenialReason').value);
            result.recommendation = document.getElementById('rotationRecommendation').value.trim();

            if (result.recommendation.length < 16 || result.recommendation.length > 256) result.recommendation = null;

            this.$emit('closeModal', {
                decision : result.decision,
                reason : result.reason || undefined,
                recommendation : result.recommendation || undefined
            });

        },
        stopPropagation: function (e) {
            e.stopPropagation()
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
.modal-block
  position: relative

.tutorial_helper
  position: absolute
  left: 0
  bottom: -100px

.modal
  background: rgba(58,72,89,.6)
  position: fixed
  z-index: 99999
  top: 0
  left: 0
  right: 0
  bottom: 0
  overflow: hidden
  display: flex
  justify-content: center
  align-items: center
  padding: 10px
  &-block
    width: 100%
    height: auto
    margin: auto 0
    border: none
    border-radius: 3px
    background: $white
    box-shadow: 0 4px 12px 0 rgba(34, 40, 46, .1)
    padding: 16px 12px
  /*&.skew-enter-to .modal-block*/
  /*animation: imageShakeIn 0.3s ease-out forwards*/
  /*&.skew-leave-to .modal-block*/
  /*animation: imageShakeOut 0.3s ease-in forwards*/
  &.skew-enter .modal-block
      transform: rotate(0) scale(0, 0)
  &.skew-leave .modal-block
    transform: rotate(0) scale(1, 1)

.denial-reason
  display: block
  width: 100%
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
