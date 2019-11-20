<template lang="pug">
  .modal(@click="closeModal(null)")
    .modal-block.card.wrapper(@click="stopPropagation")
      form.data-filter
        p За период:
        .form_line.container.container--justifyed
          label(for="dataFilterFrom") С
            span.input-wrapper
              input(type="date", name="dataFilterFrom", id="dataFilterFrom")
          label(for="dataFilterTo") по
            span.input-wrapper
              input(type="date", name="dataFilterTo", id="dataFilterTo")
        .form_line.container.container--justifyed
          button.btn.btn-default(type="submit", @click.prevent="closeModal({status:1})") ПРИМЕНИТЬ
          button.btn.btn-default.btn-reverse(type="reset", @click.prevent="closeModal({status:0})") СБРОСИТЬ

</template>

<script>
export default {
  name: "ModalDataFilter",
  props: {
    date_range : Array
  },
  data: function () {
    return {
    }
  },
  mounted: function () {
      document.getElementById('dataFilterFrom').value = this.date_range[0];
      document.getElementById('dataFilterTo').value = this.date_range[1];
  },
  methods: {
    closeModal: function (res) {
        if (res) {
            // сбросить или применить
            this.$emit('closeModal', {
                status : res.status,
                range : [
                    (new Date(document.getElementById('dataFilterFrom').value || Date.now())).getTime(),
                    (new Date(document.getElementById('dataFilterTo').value || Date.now())).getTime()
                ]
            });
        } else {
            // просто закрыта модалка
            this.$emit('closeModal');
        }

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
  .container
    display: flex !important

.data-filter
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
    align-items: center
    label
      font-weight: 600
      color: $grey
      width: calc(50% - 7px)
      text-align: left
      display: flex
      justify-content: space-between
      align-items: center
      .input-wrapper
        display: block
        overflow-x: hidden
        width: calc(100% - 20px)
        border: 1px solid $grey-lighter
        border-radius: 3px
    input
      border: none
      width: calc(100% + 40px)
      background: $grey-lighter
      padding: 10px 6px
      color: $dark
      font-size: 15px
      font-weight: 600
      text-align: center
      margin-right: -40px
    .btn
      width: calc(50% - 7px)
</style>
