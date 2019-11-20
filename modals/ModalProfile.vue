<template lang="pug">
  .modal(@click="closeModal")
    .modal-block.card.profile.wrapper(v-if='userProfileActive')
      .profile_description.container.container--justifyed.loading(v-if="loading") Профиль загружается...
      .profile_description.container.container--justifyed(v-if="user")
        .profile_text
          h2.profile_nickname {{ user.full_name }}
          p(v-html="createHtmlBiography(user.biography)")
          p.profile_follow
            span.followers <strong>{{ user.followers_count }}</strong> Подписчиков
            span.following <strong>{{ user.following_count }}</strong> Подписки
        img.profile_avatar(:src="user.avatar", :alt="user.username")
      .profile_images.container(v-if="media.length")
        .image_wrapper(v-for='image in media')
          img(:src='image.display_url', alt='Media Image', :srcset="createSrcset(image)")

</template>

<script>
export default {
  name: "ModalProfile",
  props: {
    userProfileActive: Boolean
  },
  mounted: function() {
      let self = this;


      axios({
          method : 'POST',
          url : '/api/instagram/profile/get',
          data: {
              refresh: 1
          }
      })
      .then(response => {

          self.user = response.data.user;

          self.media = response.data.media;

          self.loading = false;

      })
      .catch(res => {

      });


  },
  data: function () {
    return {
        loading : true,
        user : false,
        media : []
    }
  },
  methods: {
    createSrcset: function (media) {
        return window.createSrcset(media)
    },
    createHtmlBiography: function (text) {
      return window.createHtmlBiography(text)
    },
    closeModal: function () {
      this.$emit('userProfileActive', false)
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
.profile
  overflow: hidden
  &_description
    padding: 18px 14px 15px
    text-align: left
    .profile_avatar
      width: 78px
      height: 78px
      margin: auto 0 auto 8px
      border: 5px solid $bright-lighter
      border-radius: 50%
    .profile_text
      width: calc(100% - 86px)
      p
        text-align: left
        margin-left: 0
        margin-right: 0
        font-size: $text-small
        color: $grey-dark
    .profile_nickname
      font-size: $text-l-medium
      margin-bottom: 6px
    .profile_follow
      margin-top: 6px
      span
        display: inline-block
        color: $grey
        strong
          color: $dark
        &:not(:last-of-type)
          margin-right: 4px
  &_images
    flex-wrap: wrap
    .image_wrapper
      width: calc((100vw - 22px)/3)
      height: calc((100vw - 22px)/3)
      max-width: 152px
      max-height: 152px
      margin-top: 1px
      &:not(:nth-of-type(3n+3))
        margin-right: 1px
      img
        height: 100%
        width: 100%
        object-fit: cover
.loading
    background-image: url('/images/loading-profile-media.svg')
    background-repeat: no-repeat
    background-size: 30px
    background-position: center
    text-align: center
    display: block
    padding-bottom: 60px
</style>
