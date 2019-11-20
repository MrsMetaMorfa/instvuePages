<template lang="pug">
  .modal(@click="closeModal")
    .modal-block.card.wrapper(@click="stopPropagation")
      form.buy-shows
        p <strong>Покупка показов</strong>
        p Выберите желаемое количество
        .form_line
          div(v-if="false" v-for="price in moreShowsData.prices")
            input.u-visually-hidden(type="radio",
                name="moreShows",
                :id="'moreShows' + price[0]",
                :data-shows="price[0]"
                :data-price="price[1]")
            label.custom-radio.container(:for="'moreShows' + price[0]")
                span.radio
                span +{{ price[0] }} показов
                span.price
                  svg(xmlns="http://www.w3.org/2000/svg", viewBox="0 0 512 512")
                    path(d="M435.243,193.711c-7.943-17.294-24.785-28.037-43.952-28.037H323.08l80.803-135.489c3.68-6.168,3.761-13.837,0.214-20.081C400.549,3.858,393.921,0,386.739,0H197.113c-7.519,0-14.402,4.226-17.802,10.933L79.966,206.826c-11.117,21.828-10.111,47.329,2.69,68.214c12.803,20.885,35.069,33.353,59.565,33.353h69.865l-69.475,142.542c-9.287,18.852-3.868,40.604,13.176,52.896c7.592,5.475,16.234,8.17,24.809,8.169c10.676,0,21.247-4.178,29.542-12.365c0.362-0.358,0.712-0.73,1.045-1.114l154.695-178.034c7.231-8.322,6.346-20.929-1.975-28.16c-8.324-7.232-20.931-6.347-28.16,1.975L181.667,471.625c-0.598,0.508-1.26,0.742-2.528-0.173c-1.573-1.135-1.176-1.941-0.679-2.95l83.504-171.325c3.014-6.186,2.63-13.49-1.017-19.325c-3.648-5.836-10.044-9.381-16.927-9.381h-101.8c-10.655,0-19.959-5.21-25.527-14.295c-5.568-9.084-5.989-19.74-1.139-29.265l93.815-184.99h142.223L270.79,175.411c-3.68,6.168-3.761,13.837-0.214,20.081s10.175,10.103,17.357,10.103h103.357c4.769,0,6.856,2.994,7.676,4.779c0.839,1.826,1.771,5.447-1.419,9.162c-7.183,8.364-6.226,20.966,2.137,28.149c8.365,7.182,20.967,6.227,28.148-2.137C440.379,230.937,443.219,211.074,435.243,193.711z")
                  | {{ price[1] / 100 }}

          div
            input.u-visually-hidden(type="radio",
                name="moreShows",
                id="moreShows_range",
                :data-shows="slider_range"
                :data-price="slider_range * 100")

            label.custom-radio.container(for="moreShows_range")
              div
                span +{{ slider_range }} показов

            label.custom-radio.container(for="moreShows_range")
                span.price
                  svg(xmlns="http://www.w3.org/2000/svg", viewBox="0 0 512 512")
                    path(d="M435.243,193.711c-7.943-17.294-24.785-28.037-43.952-28.037H323.08l80.803-135.489c3.68-6.168,3.761-13.837,0.214-20.081C400.549,3.858,393.921,0,386.739,0H197.113c-7.519,0-14.402,4.226-17.802,10.933L79.966,206.826c-11.117,21.828-10.111,47.329,2.69,68.214c12.803,20.885,35.069,33.353,59.565,33.353h69.865l-69.475,142.542c-9.287,18.852-3.868,40.604,13.176,52.896c7.592,5.475,16.234,8.17,24.809,8.169c10.676,0,21.247-4.178,29.542-12.365c0.362-0.358,0.712-0.73,1.045-1.114l154.695-178.034c7.231-8.322,6.346-20.929-1.975-28.16c-8.324-7.232-20.931-6.347-28.16,1.975L181.667,471.625c-0.598,0.508-1.26,0.742-2.528-0.173c-1.573-1.135-1.176-1.941-0.679-2.95l83.504-171.325c3.014-6.186,2.63-13.49-1.017-19.325c-3.648-5.836-10.044-9.381-16.927-9.381h-101.8c-10.655,0-19.959-5.21-25.527-14.295c-5.568-9.084-5.989-19.74-1.139-29.265l93.815-184.99h142.223L270.79,175.411c-3.68,6.168-3.761,13.837-0.214,20.081s10.175,10.103,17.357,10.103h103.357c4.769,0,6.856,2.994,7.676,4.779c0.839,1.826,1.771,5.447-1.419,9.162c-7.183,8.364-6.226,20.966,2.137,28.149c8.365,7.182,20.967,6.227,28.148-2.137C440.379,230.937,443.219,211.074,435.243,193.711z")
                  | 0
                input.range(type="range", min="0", step="1", :max="Math.floor(this.$store.state.user.activity_points / 100)", id="moreShows_range_value", v-model="slider_range")
                span.price
                  svg(xmlns="http://www.w3.org/2000/svg", viewBox="0 0 512 512")
                    path(d="M435.243,193.711c-7.943-17.294-24.785-28.037-43.952-28.037H323.08l80.803-135.489c3.68-6.168,3.761-13.837,0.214-20.081C400.549,3.858,393.921,0,386.739,0H197.113c-7.519,0-14.402,4.226-17.802,10.933L79.966,206.826c-11.117,21.828-10.111,47.329,2.69,68.214c12.803,20.885,35.069,33.353,59.565,33.353h69.865l-69.475,142.542c-9.287,18.852-3.868,40.604,13.176,52.896c7.592,5.475,16.234,8.17,24.809,8.169c10.676,0,21.247-4.178,29.542-12.365c0.362-0.358,0.712-0.73,1.045-1.114l154.695-178.034c7.231-8.322,6.346-20.929-1.975-28.16c-8.324-7.232-20.931-6.347-28.16,1.975L181.667,471.625c-0.598,0.508-1.26,0.742-2.528-0.173c-1.573-1.135-1.176-1.941-0.679-2.95l83.504-171.325c3.014-6.186,2.63-13.49-1.017-19.325c-3.648-5.836-10.044-9.381-16.927-9.381h-101.8c-10.655,0-19.959-5.21-25.527-14.295c-5.568-9.084-5.989-19.74-1.139-29.265l93.815-184.99h142.223L270.79,175.411c-3.68,6.168-3.761,13.837-0.214,20.081s10.175,10.103,17.357,10.103h103.357c4.769,0,6.856,2.994,7.676,4.779c0.839,1.826,1.771,5.447-1.419,9.162c-7.183,8.364-6.226,20.966,2.137,28.149c8.365,7.182,20.967,6.227,28.148-2.137C440.379,230.937,443.219,211.074,435.243,193.711z")
                  | {{ Math.floor(this.$store.state.user.activity_points / 100) }}

        ErrorsBlock(:errors="errors")
        button.btn.btn-default(type="submit", @click.prevent="addMoreShows", id="sendButton") КУПИТЬ

</template>

<script>
import ErrorsBlock from '../ErrorsBlock.vue'

export default {
  name: "ModalMoreShows",
  components : {
      ErrorsBlock
  },
  props: {
      moreShowsData: Object
  },
  data: function () {
    return {
      slider_range: 0,
      errors : []
    }
  },
  methods: {
    addMoreShows: function ($event) {

        let button = $event.target;

        this.showErrors([]);

        /**
        if (button.classList.contains('loading')) return;

        if (!document.querySelector('input:checked')) {
            return this.showErrors("Выберите пакет показов");
        }
        */

        if (Number(document.querySelector('#moreShows_range').getAttribute('data-shows')) <= 0) {
          return this.showErrors(["Укажите количество показов"]);
        }
        
        let self = this;

        button.classList.add('loading');
        button.innerHTML = '&nbsp;';



        axios({
            method : 'POST',
            url : '/api/shop/shows/buy',
            data : {
                shows : Number(document.querySelector('#moreShows_range').getAttribute('data-shows')),
                cost : Number(document.querySelector('#moreShows_range').getAttribute('data-price')),
                for : self.moreShowsData.code
            }
        })
        .then(response => {

            // показы куплены, добавим их в UI

            self.$store.state.user.activity_points -= Number(document.querySelector('#moreShows_range').getAttribute('data-price'));
            self.$store.state.user.shows[self.moreShowsData.code] += Number(document.querySelector('#moreShows_range').getAttribute('data-shows'));

            self.$emit('closeShowsModal');

            if (!self.$store.state.tutorial.done) {
              self.$store.commit('nextStage');

              window.removeAllSelectedForTutorial();
            }

        })
        .catch(res => {
            self.showErrors(res.response.data.message);
            button.classList.remove('loading');
            button.innerHTML = 'КУПИТЬ';
        });


    },
    closeModal: function () {
        this.$emit('closeShowsModal');
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

.buy-shows
  display: block
  width: 100%
  p
    text-align: center
    width: 100%
    color: $grey
    font-weight: 600
    strong
      color: $grey
      font-weight: 600
  .form_line
    margin-top: 10px
    margin-bottom: 12px
    display: block
    width: 100%
    .custom-radio
      display: flex
      width: 100%
      //border: 1px solid $grey-lighter
      border-radius: 3px
      padding: 12px
      text-align: center
      color: $bright
      background: $white
      align-items: center
      margin-bottom: 7px
      font-weight: 600
      transition: border-color 0.3s ease-in-out
      cursor: pointer
      .radio
        border-radius: 50%
        background: $grey-lighter
        display: block
        position: relative
        width: 15px
        height: 15px
        margin-right: 8px
        &:after
          content: ''
          display: block
          background: $bright
          width: 7px
          height: 7px
          border-radius: 50%
          position: absolute
          top: 4px
          left: 4px
          opacity: 0
          transition: opacity 0.3s ease-in-out
      .price
        color: $dark
        display: flex
        margin-left: auto
        align-items: center
        font-weight: 600
        svg
          display: block
          margin-right: 4px
          height: 11px
          width: auto
          path
            fill: $dark
    input[type='radio']:checked + .custom-radio
      color: $bright
      .radio:after
        opacity: 1
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

#sendButton
  transition: background 0.3s ease-in-out, color 0.3s ease-in-out, background-position 0s
#sendButton.loading
  background-image: url('/images/loading-button.svg')
  background-repeat: no-repeat
  background-size: contain
  background-position: center

label
  div
    display: flex
    margin: 0 auto
    font-size: 15px

// полоска ползунка
$track-width: 85%
$track-height: 6px
$track-border-radius: 3px

// ползунок
$thumb-height: 15px
$thumb-width: 15px
$thumb-radius: 8px

$track-color: #424242
$thumb-color: #555bc8

input.range
  padding: 0
  -webkit-appearance: none
  margin: 0 auto
  width: $track-width

  // прячем стандатрный вид
  &::-webkit-slider-thumb
    -webkit-appearance: none

  &::-ms-track
    width: $track-width
    cursor: pointer
    background: transparent
    border-color: transparent
    color: transparent

  &:focus
    outline: none

  // начинается оформление

  // ползунок
  //WebKit/Blink
  &::-webkit-slider-thumb
    -webkit-appearance: none
    margin-top: -4px
    height: $thumb-height
    width: $thumb-width
    border-radius: $thumb-radius
    background: $bright
    cursor: pointer
  // Firefox
  &::-moz-range-thumb
    margin-top: -4px
    height: $thumb-height
    width: $thumb-width
    border-radius: $thumb-radius
    background: $bright
    cursor: pointer
  // IE
  &::-ms-thumb
    margin-top: -4px
    height: $thumb-height
    width: $thumb-width
    border-radius: $thumb-radius
    background: $bright
    cursor: pointer

  // полозка диапазона
  &::-webkit-slider-runnable-track
    width: 100%
    height: $track-height
    cursor: pointer
    background: $grey-light
    border-radius: $track-border-radius

  &:focus::-webkit-slider-runnable-track
    background: $grey-light

  &::-moz-range-track
    width: 100%
    height: $track-height
    cursor: pointer
    background: $grey-light
    border-radius: $track-border-radius

  &::-ms-track
    width: 100%
    height: $track-height
    cursor: pointer
    background: transparent
    border-color: transparent
    color: transparent

  &::-ms-fill-lower
    background: $grey-light
    border-radius: $track-border-radius

  &:focus::-ms-fill-lower
    background: $grey-light

  &::-ms-fill-upper
    background: $grey-light
    border-radius: $track-border-radius

  &:focus::-ms-fill-upper
    background: $grey-light

</style>
