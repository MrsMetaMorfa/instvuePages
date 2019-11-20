<template lang="pug">
  .modal(@click="closeModal")
    .modal-block.card.wrapper(v-if='reportActive', @click="stopPropagation")
      form.complane-reason
        p Выберите причину жалобы
        .form_line.container
          label.custom-radio(v-for="reason in reasons" :class="reason.selected ? 'checked' : ''" @click="selectReason(reason.id)")
            input.u-visually-hidden(type="radio",
                name="complaneReason",
                :value="reason.id"
                :checked="reason.selected")
            label {{ reason.text }}

        button.btn.btn-default(type="submit", @click.prevent="submitComplane") ОТПРАВИТЬ ЖАЛОБУ

</template>

<script>
export default {
  name: "ModalComplane",
  props: {
    reportActive: Boolean,
    report_data: Object
  },
  data: function () {
    return {

        reasons: [{
            id: 1,
            text: 'Спам',
            selected: false
        }, {
            id: 2,
            text: 'Хейт',
            selected: true
        }]

    }
  },
  methods: {
    selectReason: function (id) {
        this.reasons = this.reasons.map(e => Object.assign(e, {selected: false}));

        this.reasons.find(e => e.id === id).selected = true;
    },
    submitComplane: function () {

        let selected_reason = this.reasons.find(e => e.selected === true);

        if (!selected_reason) {
            return this.$emit('closeReportModal')
        }

        let api_link = '';

        if (this.report_data.type === 'recommendation') {
            api_link = '/api/account/recommendations/report';
        } else {
            api_link = '/api/account/answers/report';
        }

        axios({
            method : 'POST',
            url : api_link,
            data : {
                for : 'rotation',
                id : this.report_data.id,
                report_type : selected_reason.id
            }
        })
        .then(response => {

            this.$emit('closeReportModal');

        })
        .catch(res => {

        });

        this.$emit('closeReportModal');

    },
    closeModal: function () {
      this.$emit('closeReportModal')
    },
    stopPropagation: function (e) {
      e.stopPropagation()
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

.complane-reason
  display: block
  width: 100%
  p
    text-align: center
    width: 100%
    margin-bottom: 10px
    color: $dark
    font-weight: 600
  .form_line
    margin-bottom: 12px
    display: block
    width: 100%
    justify-content: space-between
    .custom-radio
      display: block
      width: calc(50% - 6px)
      border: 1px solid transparent
      border-radius: 3px
      padding: 12px
      text-align: center
      color: $grey
      background: $grey-lighter
    .custom-radio.checked
      border-color: $bright
      color: $bright
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
.container
    display: flex !important
</style>
