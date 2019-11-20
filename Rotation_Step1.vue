<template lang="pug">
.rotation_avatars.wrapper
  .tutorial_helper(v-if="showTutorialHelper")
    p Здесь <span> пользователи</span> демонстрируют свои Instagram аккаунты,<br> <span>получают подписчиков</span> по интересам, <span>статистику</span> и<br> обратную связь с <span>рекомендациями</span> по улучшению своего профиля и контента
    button(@click="nextStage" class="btn btn-default") Попробовать &#128522;

  p.hint Выберите 3 аватара, которые вам больше всего интересны
  .container.container--justifyed
    .avatar(v-for="candidate in this.$store.state.rotation.candidates")
      input.u-visually-hidden(type='checkbox', name="avatar", :id='candidate.uid', ischeck='uncheck', @change="avatarCheck")
      label(:for="candidate.uid").image_wrapper
        img(:src="candidate.avatar", :alt='candidate.username')
        .checkmark
          svg(xmlns="http://www.w3.org/2000/svg", viewBox="0 0 11 9")
            path(fill-rule="evenodd", fill="rgb(255, 255, 255)", d="M0.555,4.133 L1.613,2.926 L5.897,6.668 L4.839,7.875 L0.555,4.133 Z")
            path(fill-rule="evenodd", fill="rgb(255, 255, 255)", d="M5.450,8.319 L4.215,7.292 L9.571,0.878 L10.806,1.905 L5.450,8.319 Z")
</template>

<script>
export default {
    name: 'Rotation_Step1',
    data: function () {
        return {
            itemChecked: 0,
            progressPercent: 0
        }
    },
    computed: {
      showTutorialHelper: function() {
        return !this.$store.state.tutorial.done
          && this.$store.state.tutorial.substage === 1
          && this.$store.state.tutorial.step === 2;
      }
    },
    methods: {
        nextStage: function () {
          this.$store.commit('nextStage');

          window.removeAllSelectedForTutorial();
          window.selectForTutorial('.rotation_avatars .avatar', true);
          window.selectForTutorial('main .hint', false);
        },
        avatarCheck: function ($event) {
            let inputArray = document.querySelectorAll('.avatar input');

            if ($event.target.getAttribute('ischeck') === 'uncheck') {

                $event.target.setAttribute('ischeck', 'checked');

                if (this.itemChecked < 3) {

                    this.itemChecked++;
                    this.progressPercent = this.itemChecked * (100 / 9);

                    this.$emit('progressPercent', this.progressPercent);

                    if (this.itemChecked === 3) {

                        let selected_candidates = [];

                        document.querySelectorAll('.avatar input:checked').forEach(element => {
                            selected_candidates.push(Number(element.getAttribute('id')));
                        });

                        this.$emit('step1Ended', selected_candidates);

                        for (let n = 0; n < inputArray.length; n++) {
                            if (inputArray[n].getAttribute('ischeck') !== 'checked') {
                                inputArray[n].setAttribute('disabled', true);
                            }
                        }
                    }

                }

            } else if ($event.target.getAttribute('ischeck') === 'checked') {

                $event.target.classList.add('remove-check');

                setTimeout(function () {
                    $event.target.classList.remove('remove-check');
                }, 600);

                $event.target.setAttribute('ischeck', 'uncheck');

                if (this.itemChecked === 3) {
                    for (let n = 0; n < inputArray.length; n++) {
                        inputArray[n].setAttribute('disabled', false);
                    }
                }

                this.itemChecked = this.itemChecked - 1;
                this.progressPercent = this.itemChecked * (100 / 9);

                this.$emit('progressPercent', this.progressPercent);

            }
        }
    }
}
</script>

<style scoped lang="sass">
@import "../../sass/settings.scss"

.tutorial_helper
  position: absolute
  bottom: -30px
  button
    margin-top: 10px

.rotation_avatars
  position: relative
  .hint
    text-align: center
    color: $grey
    font-weight: 600
    margin: 0 8% 0
  .container
    flex-wrap: wrap
    max-width: 456px
    margin: 0 auto
    .avatar
      width: calc(100% / 3)
      margin-bottom: 12px
      .image_wrapper
        margin: 0 auto
        padding: 16px 12px
        display: block
        width: 100%
        position: relative
        img
          width: calc(((100vw - 32px)/3) - 24px)
          height: calc(((100vw - 32px)/3) - 24px)
          max-width: 128px
          max-height: 128px
          background: $white
          border-radius: 50%
          border: 4px solid $white
          box-shadow: 0 4px 12px 0 rgba(34, 40, 46, 0.1)
      .checkmark
        position: absolute
        bottom: 12px
        left: 50%
        margin-left: -12px
        border: 3px solid $white
        border-radius: 50%
        background: $bright
        padding: 5px 3px
        width: 24px
        height: 24px
        display: flex
        justify-content: center
        align-items: center
        transform: scale(0)
        transition: transform 0.3s ease-in-out
        svg
          width: auto
          height: 100%
      input:checked + .image_wrapper
        animation: itemTap 0.3s ease-in
        .checkmark
          animation: itemTapIcon 0.5s ease-out forwards
      input.remove-check + .image_wrapper
        animation: itemTapInvert 0.3s ease-in
        .checkmark
          animation: itemTapIconInvert 0.5s ease-in forwards

</style>
