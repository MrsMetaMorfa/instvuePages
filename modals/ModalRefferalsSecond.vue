<template lang="pug">
  .modal(@click="closeModal")
    .modal-block.card.profile.wrapper(@click.prevent)
      div.popup_ref_counter
        button.popup_close &times;
        div.ref_counter 5,943
        div.ref_counter__text Количество рефералов
        div.points_counter
          span.points_counter__text Баллов заработано:
          span.points_counter__value 485

</template>

<script>
export default {
  name: "ModalRefferalsSecond",
  methods: {
    closeModal: function ($event) {

      // закрывать только по клику вне модалки
      if (!$event.target || !$event.target.classList.contains('modal')) return;

      this.$emit('closeModal');

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
    padding: 0
    transition: transform 0.3s ease-in-out
  &.skew-enter, &.skew-leave-to
    transform: scale(1)
  &.skew-leave, &.skew-enter-to
    transform: scale(1)
  &.skew-enter .modal-block, &.skew-leave-to .modal-block
    transform: scale(0)
  &.skew-leave .modal-block, &.skew-enter-to .modal-block
    transform: scale(1)


</style>
<style scoped lang='scss'>
  .ref_counter {
    font-size: 30px;
    font-weight: 700;
    margin-bottom: 5px;
  }
  .ref_counter__text {
    font-size: 10px;
    font-weight: 600;
    color: #ffffff !important;
    margin-bottom: 5px;
  }
  .points_counter__text {
    font-size: 10px;
    color: #ffffff !important;
    display: inline-block;

  }
  .points_counter__value {
    font-size: 12px;
    font-weight: 700;
    color: #ffffff !important;
    display: inline-block;
    margin-left: 7px;
  }
  .popup_ref_counter {
    border-radius: 15px;
    background:url("data:image/svg+xml,%3C%3Fxml version='1.0' encoding='iso-8859-1'%3F%3E%3C!-- Generator: Adobe Illustrator 19.0.0, SVG Export Plug-In . SVG Version: 6.00 Build 0) --%3E%3Csvg version='1.1' id='Capa_1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink' x='0px' y='0px' viewBox='0 0 512 512' style='enable-background:new 0 0 512 512;' xml:space='preserve' width='15px' height='15px' fill='%23a7b7c3'%3E%3Cpath d='M234.071,471.132H60.391c-6.453,0-12.333-2.991-16.135-8.207c-3.803-5.218-4.85-11.736-2.874-17.883 c19.732-61.346,79.908-104.191,146.336-104.191c30.909,0,60.591,9.308,85.838,26.916c9.043,6.307,21.485,4.09,27.795-4.953 c6.306-9.043,4.089-21.486-4.954-27.794c-12.498-8.717-25.85-15.828-39.817-21.257c18.583-16.896,30.911-40.555,33.053-67.048 c28.177-27.448,65.111-42.488,104.704-42.488c30.909,0,60.591,9.308,85.838,26.916c9.043,6.307,21.486,4.09,27.795-4.953 c6.306-9.043,4.089-21.486-4.954-27.794c-12.498-8.717-25.85-15.828-39.817-21.257c20.499-18.638,33.386-45.506,33.386-75.328 C496.586,45.673,450.913,0,394.774,0c-56.14,0-101.812,45.673-101.812,101.813c0,29.701,12.784,56.473,33.139,75.102 c-2.785,1.072-5.55,2.212-8.295,3.42c-12.492,5.497-24.241,12.245-35.162,20.183c-15.068-37.415-51.746-63.893-94.49-63.893 c-56.14,0-101.812,45.673-101.812,101.813c0,29.614,12.71,56.316,32.96,74.938c-54.148,20.292-98.053,63.87-115.927,119.444 c-5.928,18.431-2.788,37.976,8.616,53.623c11.402,15.645,29.042,24.618,48.401,24.618h173.68 c11.026,0,19.963-8.938,19.963-19.963S245.096,471.132,234.071,471.132z M394.775,39.926c34.124,0,61.886,27.762,61.886,61.886 s-27.762,61.886-61.886,61.886c-34.124,0-61.886-27.762-61.886-61.886S360.651,39.926,394.775,39.926z M188.155,176.55 c34.124,0,61.886,27.762,61.886,61.886s-27.762,61.886-61.886,61.886s-61.886-27.762-61.886-61.886 S154.031,176.55,188.155,176.55z'/%3E%3Cpath d='M503.217,326.082c-8.965-6.418-21.436-4.354-27.853,4.612l-98.4,137.447c-2.687,3.116-6.055,3.789-7.859,3.909 c-1.857,0.127-5.463-0.114-8.555-3.057l-63.703-61.168c-7.954-7.638-20.593-7.379-28.226,0.573 c-7.637,7.952-7.38,20.59,0.572,28.226l63.767,61.228c9.55,9.091,22.298,14.149,35.414,14.149c1.127,0,2.257-0.037,3.387-0.113 c14.288-0.952,27.628-7.9,36.599-19.062c0.233-0.289,0.455-0.584,0.672-0.885l98.799-138.006 C514.247,344.97,512.183,332.5,503.217,326.082z'/%3E%3C/svg%3E%0A") no-repeat, url(../../../images/partnership/popup1_bg.jpg);
    background-size: auto, cover;
    background-position: center 15px, center;
    padding-top: 45px !important;
    padding-bottom: 25px !important;
    color: #3a4859 !important;
    text-align: center;
    position: relative;
      .ref_counter {
        font-weight: 400 !important;
      }
      .ref_counter__text,.points_counter__text,.points_counter__value {
        color: #a7b7c3 !important;
      }
  }
  .popup_close {
    color: #a7b7c3;
    position: absolute;
    top: 5px;
    right: 5px;
    background: none;
    border:none;
    font-size: 10px;
    cursor: pointer;
  }
</style>
