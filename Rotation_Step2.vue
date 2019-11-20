<template lang="pug">
.rotation_profiles.wrapper
  p.hint Выберите 1 профиль, описание которого вас больше всего заинтересовало
  .container.container--column
    .profile(v-for="candidate in this.$store.state.rotation.candidates")
      input.u-visually-hidden(type='checkbox', name="profile", :id='candidate.uid', @change="profileCheck")
      label(:for="candidate.uid")
        .profile_image
          img(:src="candidate.avatar", :alt='candidate.username')
        .profile_description
          h2.profile_name {{ candidate.full_name }}
          p.profile_text(v-html="createHtmlBiography(candidate.biography)")
        .checkmark
          svg(xmlns="http://www.w3.org/2000/svg", viewBox="0 0 11 9")
            path(fill-rule="evenodd", fill="rgb(255, 255, 255)", d="M0.555,4.133 L1.613,2.926 L5.897,6.668 L4.839,7.875 L0.555,4.133 Z")
            path(fill-rule="evenodd", fill="rgb(255, 255, 255)", d="M5.450,8.319 L4.215,7.292 L9.571,0.878 L10.806,1.905 L5.450,8.319 Z")
</template>

<script>
export default {
    name: 'Rotation_Step2',
    data: function () {
        return {
            progressPercent: (100 / 3)
        }
    },
    methods: {
        createHtmlBiography: function (text) {
            return window.createHtmlBiography(text)
        },
        profileCheck: function () {
            let profileArray = document.querySelectorAll('.profile input');

            for (let n = 0; n < profileArray.length; n++) {
                profileArray[n].setAttribute('disabled', true);
            }

            this.progressPercent = this.progressPercent + (100 / 3);

            this.$emit('progressPercent', this.progressPercent);
            this.$emit('step2Ended', Number(document.querySelectorAll('.profile input:checked')[0].getAttribute('id')));
        }
    }
}
</script>

<style scoped lang="sass">
@import "../../sass/settings.scss"
.rotation_profiles
  position: relative
  padding-bottom: 10px
  .hint
    text-align: center
    color: $grey
    font-weight: 600
    margin: 0 8% 16px
  .container
    flex-wrap: wrap
    max-width: 456px
    margin: 0 auto
    .profile
      width: 100%
      margin-bottom: 12px
      label
        flex-wrap: nowrap
        position: relative
        flex-direction: row
        display: flex
        padding: 12px 19px
        background: $white
        border: 1px solid $bright-lighter
        border-radius: 3px
        box-shadow: 0 4px 12px 0 rgba(34, 40, 46, 0.1)
        .profile_image
          margin: auto 19px auto 0
          width: 55px
          img
            width: 55px
            height: 55px
            border-radius: 50%
            border: 4px solid $bright-lighter
            background: $bright-lighter
            box-shadow: 0 4px 12px 0 rgba(34, 40, 46, 0.1)
        .profile_description
          width: calc(100% - 74px)
        .profile_name
          margin-bottom: 5px
        .profile_text
          font-size: $text-small
          line-height: 1.3
          color: $grey
        .checkmark
          position: absolute
          top: 50%
          right: 19px
          margin-top: -12px
          border: 3px solid $white
          border-radius: 50%
          background: $bright
          width: 24px
          height: 24px
          padding: 5px 3px
          display: flex
          justify-content: center
          align-items: center
          transform: scale(0)
          transition: transform 0.3s ease-in-out
          svg
            width: auto
            height: 100%
      input:checked + label
        animation: itemTap 0.3s ease-in
        border-color: $bright
        transition: border-color 0.3s ease-in
        .checkmark
          animation: itemTapIcon 0.5s ease-out forwards
</style>
