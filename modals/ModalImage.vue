<template lang="pug">
  .modal.modal-image(@click="closeModal")
    div.wrapper
      img(v-if="media.type !== 2", :src="media.display_url", alt="Media")
      video(v-if="media.type === 2" controls, id="video_media") Ваш браузер не поддерживает Video тэг
        source(:src="media.video_url" type="video/mp4")

      p.caption(ref="caption" v-if="media.caption" v-html="createHtmlBiography(media.caption, media.username)")
</template>

<script>
export default {
  name: "ModalImage",
  props: {
    media : {
      type: Object
    },
    modalOpen: Boolean
  },
  mounted: function() {
    let self = this;

    let a_tag = this.$refs['caption'].getElementsByTagName('a');

    if (!a_tag.length) return;

    a_tag[0].addEventListener('click', function(event) {

      self.$store.commit('profileWasOpened', 1);

      if (!self.$store.state.tutorial.done) {
        self.$store.commit('nextStage', 4);
      }

    });

  },
  methods: {
    createHtmlBiography: function (text, username) {
      if (username) {
        return window.createHtmlBiography(text, username)
      } else {
        return window.createHtmlBiography(text)
      }
    },
    closeModal: function ($event) {

      if ($event.target && $event.target.localName === 'a') return; // не закрывать при клике на ссылку

      this.$emit('closeModal', false)
      $event.target.classList.remove('open')
      setTimeout(function () {
        this.media = {}
      }, 300)
    }
  }
}
</script>

<style scoped lang="sass">
@import "../../../sass/settings.scss"
.modal::-webkit-scrollbar
  width: 0 !important
.modal
  display: flex
  flex-flow: wrap
  justify-content: center
  align-items: center
  position: absolute
  z-index: 1500
  top: 0
  left: 0
  right: 0
  bottom: 0
  padding: 10px
  overflow: auto
  overflow: -moz-scrollbars-none
  -ms-overflow-style: none

  div
    text-align: center
    background: $white
    padding: 10px 0
    .caption
      text-align: left
      margin-top: 10px
      padding: 0 4%
  &-image
    img, video
      max-width: 95%
      max-height: 95%
      margin: auto 0
      display: initial
    /*&.skew-enter-to > img*/
    /*animation: imageShakeIn 0.3s ease-out forwards*/
    /*&.skew-leave-to > img*/
    /*animation: imageShakeOut 0.3s ease-in forwards*/
    &.skew-enter > img
        transform: rotate(0) scale(0, 0)
    &.skew-leave > img
      transform: rotate(0) scale(1, 1)
</style>
