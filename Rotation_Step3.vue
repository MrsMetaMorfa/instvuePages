<template lang="pug">
.rotation_winner.wrapper
  p.winner_title.container
    svg(xmlns="http://www.w3.org/2000/svg", viewBox="0 0 512 512")
      path(d="M483.624,28.855c3.062-6.199,2.704-13.54-0.945-19.412S472.606,0,465.692,0H336.309c-7.612,0-14.563,4.32-17.933,11.145l-79.841,161.677c-29.485,2.901-56.784,13.366-79.922,29.425L78.491,40h84.772l32.114,65.031c4.891,9.904,16.883,13.967,26.788,9.077c9.904-4.891,13.968-16.884,9.077-26.788l-37.617-76.175C190.255,4.32,183.303,0,175.692,0H46.309c-6.914,0-13.338,3.571-16.988,9.443c-3.649,5.872-4.007,13.213-0.945,19.412l98.766,200c0.165,0.335,0.339,0.663,0.52,0.984C101.31,259.793,85.309,299.063,85.309,342c0,93.738,76.262,170,170,170c31.268,0,61.841-8.567,88.413-24.776c9.43-5.752,12.411-18.059,6.659-27.489c-5.753-9.431-18.06-12.413-27.489-6.66c-20.297,12.38-43.667,18.924-67.585,18.924c-71.683,0-130-58.318-130-130s58.317-130,130-130s130,58.318,130,130c0,17.11-3.266,33.729-9.707,49.396c-4.199,10.216,0.678,21.903,10.894,26.103c10.216,4.2,21.904-0.678,26.103-10.894c8.435-20.516,12.711-42.252,12.711-64.604c0-42.49-15.67-81.39-41.533-111.223c0.392-0.614,0.753-1.255,1.083-1.922L483.624,28.855z M353.032,202.966c-20.626-14.54-44.616-24.612-70.551-28.796L348.738,40h84.771L353.032,202.966z")
      path(d="M256.309,414c11.046,0,20-8.954,20-20V292c0-11.046-8.954-20-20-20h-35c-11.046,0-20,8.954-20,20c0,11.046,8.954,20,20,20h15v82C236.309,405.046,245.263,414,256.309,414z")
    span Победитель ротации
    a.link(target="_blank", :href="'https://www.instagram.com/' + this.$store.state.rotation.preview.user.username", @click="toggleVisibilityFollowButtons") @{{ this.$store.state.rotation.preview.user.username }}
  .winner_message.container.container--justifyed
    img.winner_avatar(:src="this.$store.state.rotation.preview.user.avatar", :alt="this.$store.state.rotation.preview.user.username")
    .message_wrapper(v-if="this.$store.state.rotation.appeal_message")
      p {{ this.$store.state.rotation.appeal_message.text }}
      button.btn.container.container--justifyed(type="button", v-if="!this.$store.state.rotation.has_answer", @click="toggleVisibilityComment")
        span.link Ответить
        | +
        svg(xmlns="http://www.w3.org/2000/svg", viewBox="0 0 512 512")
          path(d="M435.243,193.711c-7.943-17.294-24.785-28.037-43.952-28.037H323.08l80.803-135.489c3.68-6.168,3.761-13.837,0.214-20.081C400.549,3.858,393.921,0,386.739,0H197.113c-7.519,0-14.402,4.226-17.802,10.933L79.966,206.826c-11.117,21.828-10.111,47.329,2.69,68.214c12.803,20.885,35.069,33.353,59.565,33.353h69.865l-69.475,142.542c-9.287,18.852-3.868,40.604,13.176,52.896c7.592,5.475,16.234,8.17,24.809,8.169c10.676,0,21.247-4.178,29.542-12.365c0.362-0.358,0.712-0.73,1.045-1.114l154.695-178.034c7.231-8.322,6.346-20.929-1.975-28.16c-8.324-7.232-20.931-6.347-28.16,1.975L181.667,471.625c-0.598,0.508-1.26,0.742-2.528-0.173c-1.573-1.135-1.176-1.941-0.679-2.95l83.504-171.325c3.014-6.186,2.63-13.49-1.017-19.325c-3.648-5.836-10.044-9.381-16.927-9.381h-101.8c-10.655,0-19.959-5.21-25.527-14.295c-5.568-9.084-5.989-19.74-1.139-29.265l93.815-184.99h142.223L270.79,175.411c-3.68,6.168-3.761,13.837-0.214,20.081s10.175,10.103,17.357,10.103h103.357c4.769,0,6.856,2.994,7.676,4.779c0.839,1.826,1.771,5.447-1.419,9.162c-7.183,8.364-6.226,20.966,2.137,28.149c8.365,7.182,20.967,6.227,28.148-2.137C440.379,230.937,443.219,211.074,435.243,193.711z")

      button.btn.container.container--justifyed(type="button", v-if="this.$store.state.rotation.has_answer")
        span.answered Отвечено

  .winner_buttons.container.container--justifyed
    ErrorsBlock(:errors="errors")
    form.winner_comment(v-if='visibleComment')
      input(type="text", id="answerMessage", name="answerMessage", placeholder='Напишите свой ответ...', @focus="focusedInput")
      button.btn(type="submit", @click.prevent="sendAnswer", @focus="focusedInput") Отправить
        svg(xmlns="http://www.w3.org/2000/svg", viewBox="0 0 60 56.4")
          path(d="M1.2,55.2A3.5,3.5,0,0,1,0,53.3c0-.4,1.7-6.5,3.7-13.7,3.4-11.9,3.8-13,4.8-13.9l1-1H25.8L27,25.9a3.1,3.1,0,0,1,1.2,2.3A3.2,3.2,0,0,1,27,30.6l-1.2,1.1H13.2l-2,7.2c-1.1,3.9-2,7.1-1.9,7.2S47.6,28.5,47.6,28.2,9.1,9.9,8.9,10a19.6,19.6,0,0,0,.9,4c1,4.4.9,5.5-1,6.6a3.2,3.2,0,0,1-4.7-1.3A158.3,158.3,0,0,1,0,3.2C0,2.1,2.2,0,3.3,0S16.5,5.7,31.5,12.7C60.2,26.1,60,26,60,28.2s.3,2.1-28.5,15.5c-15,7-27.7,12.7-28.2,12.7a3.3,3.3,0,0,1-2.1-1.2Z")
    a.btn.btn-default.winner_view(type="button", target="_blank", :href="'https://www.instagram.com/' + this.$store.state.rotation.preview.user.username", v-if='!wasProfileOpen', @click="toggleVisibilityFollowButtons") ПОСМОТРЕТЬ ПРОФИЛЬ
    button.btn.btn-default.btn-reverse.winner_denied(type="button", v-if='wasProfileOpen', @click="setDenialVisible")
        | НЕ ПОДПИСАЛСЯ
    button.btn.btn-default.container.winner_access(type="button", v-if='wasProfileOpen', @click="onCloseModal({decision:1})")
      svg(xmlns="http://www.w3.org/2000/svg", viewBox="0 0 512 512")
        path(d="M435.243,193.711c-7.943-17.294-24.785-28.037-43.952-28.037H323.08l80.803-135.489c3.68-6.168,3.761-13.837,0.214-20.081C400.549,3.858,393.921,0,386.739,0H197.113c-7.519,0-14.402,4.226-17.802,10.933L79.966,206.826c-11.117,21.828-10.111,47.329,2.69,68.214c12.803,20.885,35.069,33.353,59.565,33.353h69.865l-69.475,142.542c-9.287,18.852-3.868,40.604,13.176,52.896c7.592,5.475,16.234,8.17,24.809,8.169c10.676,0,21.247-4.178,29.542-12.365c0.362-0.358,0.712-0.73,1.045-1.114l154.695-178.034c7.231-8.322,6.346-20.929-1.975-28.16c-8.324-7.232-20.931-6.347-28.16,1.975L181.667,471.625c-0.598,0.508-1.26,0.742-2.528-0.173c-1.573-1.135-1.176-1.941-0.679-2.95l83.504-171.325c3.014-6.186,2.63-13.49-1.017-19.325c-3.648-5.836-10.044-9.381-16.927-9.381h-101.8c-10.655,0-19.959-5.21-25.527-14.295c-5.568-9.084-5.989-19.74-1.139-29.265l93.815-184.99h142.223L270.79,175.411c-3.68,6.168-3.761,13.837-0.214,20.081s10.175,10.103,17.357,10.103h103.357c4.769,0,6.856,2.994,7.676,4.779c0.839,1.826,1.771,5.447-1.419,9.162c-7.183,8.364-6.226,20.966,2.137,28.149c8.365,7.182,20.967,6.227,28.148-2.137C440.379,230.937,443.219,211.074,435.243,193.711z")
      | ПОДПИСАЛСЯ

  .tutorial_helper(v-if="showTutorialHelper")
    p(v-html="tutorialHeplerHtml")

  .winner_profile
    .winner_description.container.container--justifyed
      .winner_text
        h2.winner_nickname {{ this.$store.state.rotation.preview.user.full_name }}
        p(v-html="createHtmlBiography(this.$store.state.rotation.preview.user.biography)")
        p.winner_follow
          span.followers <strong>{{ this.$store.state.rotation.preview.user.followers_count }}</strong> Подписчиков
          span.following <strong>{{ this.$store.state.rotation.preview.user.following_count }}</strong> Подписки
      img.winner_avatar(:src="this.$store.state.rotation.preview.user.avatar", alt="this.$store.state.rotation.preview.user.username")
    .winner_images.container
      .image_wrapper(v-for='image in this.$store.state.rotation.preview.media')
        img(:src='image.display_url', :srcset="createSrcset(image)", alt='Winner Post Image', @click="openModalImage(image)")
        div.video(v-if="image.type === 2")

  button.btn.btn-default.new-rotation(type="button", @click="onCloseModal({decision:3})") НОВАЯ РОТАЦИЯ

  ModalBasic(v-if="visibleDenial" @closeModal="onCloseModal")
</template>

<script>
import ModalBasic from './modals/ModalBasic'
import ErrorsBlock from './ErrorsBlock.vue'

export default {
    name: 'Rotation_Step3',
    components: {
        ModalBasic,
        ErrorsBlock
    },
    data: function () {
        return {
          progressPercent: (2 * 100 / 3),
          visibleComment: false,
          visibleDenial: false,
          errors: []
        }
    },
    computed: {
      wasProfileOpen: function() {
        console.log(this.$store.state.rotation.was_profile_opened);
        return this.$store.state.rotation.was_profile_opened === 1;
      },
      showTutorialHelper: function() {
        return !this.$store.state.tutorial.done
          && this.$store.state.tutorial.step === 4;
      },
      tutorialHeplerHtml: function() {
        if (!this.$store.state.rotation.was_profile_opened && this.$store.state.tutorial.substage === 4) {
          this.$store.state.tutorial.substage = 3;
          return 'Давай перейдем к победителю в <span>Instagram</span><br>Когда ознакомишься с его профилем, возвращайся обратно';
        }

        switch (this.$store.state.tutorial.substage) {
          case 2:
            return 'Это профиль победителя. <span>Можно нажать</span> на любое фото или видео';
            break;

          case 3:
            return 'Давай перейдем к победителю в <span>Instagram</span><br>Когда ознакомишься с его профилем, возвращайся обратно';
            break;

          case 4:
            window.removeAllSelectedForTutorial();
            window.selectForTutorial('.winner_buttons', false);
            return 'После поcещения профиля <span>выберите</span> ваше решение';
            break;
        }

      }
    },
    methods: {
        createHtmlBiography: function (text) {
            return window.createHtmlBiography(text)
        },
        // отправить свой ответ
        sendAnswer: function() {
            this.errors = [];

            let message = document.getElementById('answerMessage').value.trim();

            if (message.length < 8 || message.length > 256) {
                return this.showErrors("Длина сообщения должна быть от 8 до 256 символов")
            }

            let self = this;

            axios({
                method : 'POST',
                url : '/api/rotation/answer',
                data : {
                    rotation_id : this.$store.state.rotation.rotation_id,
                    appeal_message_id : this.$store.state.rotation.appeal_message.id,
                    appeal_message_hash : window.md5(this.$store.state.rotation.appeal_message.text),
                    answer : message
                }
            })
            .then(response => {
                self.visibleComment = false;
                self.$store.state.rotation.has_answer = true;
            })
            .catch(res => {
                self.showErrors(res.response.data.message);
            });

        },
        showErrors : function (errors) {
            if (typeof errors === 'string') {
                errors = [errors];
            }

            this.errors = errors;
        },
        setDenialVisible: function () {
            this.visibleDenial = true;
        },
        // не подписался. в result вся нужная информация
        onCloseModal: function (result) {

            if (!result) {
                this.visibleDenial = false;
                return;
            }

            this.$emit('step3Ended', Object.assign(result));
        },
        profileCheck: function () {

            this.progressPercent = this.progressPercent + (100 / 3)
            this.$emit('progressPercent', this.progressPercent)

        },
        toggleVisibilityComment: function () {

            if (document.querySelector('.winner_comment')) {

                document.querySelector('.winner_comment').classList.toggle('show')

                setTimeout(function () {
                    this.visibleComment = !this.visibleComment
                }, 200)

            } else {

                this.visibleComment = !this.visibleComment

                setTimeout(function () {
                    document.querySelector('.winner_comment').classList.toggle('show')
                }, 200)

            }

        },
        toggleVisibilityFollowButtons: function () {
            // добавим статус того что профиль был открыт
            this.$store.commit('profileWasOpened', 1);

            // если проходится обучение, то пропустим шаг
            if (!this.$store.state.tutorial.done) {
              this.$store.commit('nextStage');
            }

            this.$forceUpdate();
        },
        focusedInput: function ($event) {
            let form = $event.target

            while (form.tagName !== 'FORM') {
                form = form.parentElement
            }

            form.classList.add('active')
        },
        openModalImage: function (image) {
          if (!this.$store.state.tutorial.done) {
            this.$store.commit('nextStage');
            window.removeAllSelectedForTutorial();
            window.selectForTutorial('.winner_view', false);
          }

          this.$emit('openImage', Object.assign(image, {username: this.$store.state.rotation.preview.user.username}))

        },
        openDenialReason: function () {
            this.$emit('openDenialReason', true)
        },
        createSrcset: function (media) {
            return window.createSrcset(media)
        }
    },
  directives: {
    // you can delete it if it don't need
    longpress: {
      bind: function (el, binding, vNode) {
        // Make sure expression provided is a function
        if (typeof binding.value !== 'function') {
          // Fetch name of component
          const compName = vNode.context.name
          // pass warning to console
          let warn = `[longpress:] provided expression '${binding.expression}' is not a function, but has to be`
          if (compName) { warn += `Found in component '${compName}' ` }

          //console.warn(warn)
        }

        // Define variable
        let pressTimer = null

        // Define function handlers
        // Create timeout ( run function after 1s )
        let start = (e) => {
          if (e.type === 'click' && e.button !== 0) {
            return
          }
          if (pressTimer === null) {
            pressTimer = setTimeout(() => {
              // Run function
              handler()
            }, 100)
          }
        }

        // Cancel Timeout
        let cancel = (e) => {
          // Check if timer has a value or not
          if (pressTimer !== null) {
            clearTimeout(pressTimer)
            pressTimer = null
          }
        }
        // Run Function
        const handler = (e) => {
          binding.value(e)
          //console.log(el.getAttribute('src'))
          //console.log(el.getAttribute('alt'))
          vNode.context.$emit('openImage', {
            imgsrc: el.getAttribute('src'),
            imgalt: el.getAttribute('alt')
          })
        }

        // Add Event listeners
        el.addEventListener("mousedown", start)
        el.addEventListener("touchstart", start)
        // Cancel timeouts if this events happen
        el.addEventListener('scroll', cancel)
        el.addEventListener('mousemove', cancel)
        el.addEventListener('touchmove', cancel)
        el.addEventListener("click", cancel)
        el.addEventListener("mouseout", cancel)
        el.addEventListener("touchend", cancel)
        el.addEventListener("touchcancel", cancel)
      }
    }
  }
}
</script>

<style scoped lang="sass">
@import "../../sass/settings.scss"
.loading_wrapper
    height: 100px
    width: 100%
    padding-top: 10px
    text-align: center
    background-image: url('/images/loading-profile-media.svg')
    background-color: #f2f3f5
    background-repeat: no-repeat
    background-size: 20%
    background-position: center

.rotation_winner
  position: relative
  margin: 0 auto
  padding-bottom: 10px
  .winner_buttons
    margin-top: 15px
  p
    text-align: center
    color: $grey
    font-weight: 600
    margin: 0 8% 0
    &.winner_title
      justify-content: center
      align-items: center
      margin-bottom: 12px
      svg
        height: 12px
        width: auto
        margin-left: 3px
        margin-right: 3px
        path
          fill: $grey
      span
        display: inline-block
        margin-left: 3px
        margin-right: 3px
        &.link
          color: $bright
  .container
    flex-wrap: wrap
  .new-rotation
    text-align: center
    background: $bright-light
    border-color: $bright-light
    font-size: $text-small
    font-weight: 700
    &:hover, &:focus
      color: $white
      background: $bright
      border-color: $bright
.winner
  &_message
    align-items: center
    .winner_avatar
      margin-top: -5%
    .message_wrapper
      display: block
      padding: 8px 10px 4px
      border-radius: 5px
      background: $white
      width: calc(100% - 74px)
      margin-left: auto
      margin-bottom: 16px
      box-shadow: 0 4px 12px 0 rgba(34, 40, 46, .1)
      position: relative
      &:before
        content: ''
        position: absolute
        background-image: -webkit-radial-gradient(1px 2px, 18px 20px, transparent 11px, #fff 11px)
        width: 16px
        height: 16px
        border-radius: 50%
        z-index: 0
        bottom: 0
        left: -12px
      p
        margin: 0
        font-size: $text-small
        color: $dark
        text-align: left
        font-weight: 500
      .btn
        margin-left: auto
        margin-top: 5px
        display: flex
        font-size: $text-x-small
        color: $grey
        height: 10px
        align-items: center
        span
          display: inline-block
          border-bottom: 1px dotted
          margin-right: 3px
        span.answered
          border: none !important
        svg
          height: 100%
          width: auto
          margin-left: 2px
          path
            fill: $bright
  &_avatar
    width: 56px
    height: 56px
    border-radius: 50%
    border: 5px solid $white
  &_buttons
    margin-bottom: 20px
    .winner_view
      width: 100%
      font-size: $text-small
      font-weight: 700
      text-align: center
    .winner_denied, .winner_access
      width: calc(50% - 8px)
      font-size: $text-small
      font-weight: 700
    .winner_access
      display: flex
      justify-content: center
      svg
        height: 9px
        width: auto
        margin-right: 3px
        path
          fill: $white
      &:hover, &:focus
        svg path
          fill: $bright
    .winner_comment
      width: 100%
      border: solid $bright-light
      border-radius: 3px
      background: $white
      display: flex
      overflow: hidden
      align-items: flex-start
      margin-bottom: 0
      height: 0
      border-width: 0 1px 0 1px
      opacity: 0
      transition: height 0.2s ease-in-out, border-width 0.1s ease-out 0.1s, opacity 0.2s ease-in-out
      &.show
        margin-bottom: 16px
        height: 33px
        border-width: 1px 1px 1px 1px
        opacity: 1
      &.active
        border-color: $bright
      input
        padding: 8px
        width: calc(100% - 28px)
        border: none
        background: transparent
      .btn
        padding: 10px 8px
        font-size: 0
        line-height: 0
        display: flex
        justify-content: center
        align-items: center
        svg
          width: 12px
          height: auto
          path
            fill: $grey
  &_profile
    margin-bottom: 15px
    border-radius: 3px
    background: $white
    overflow: hidden
    box-shadow: 0 4px 12px 0 rgba(34, 40, 46, 0.1)
    span.link
      color: $bright
  &_description
    padding: 18px 14px 15px
    text-align: left
    .winner_avatar
      width: 78px
      height: 78px
      margin: auto 0 auto 8px
      border: 5px solid $bright-lighter
    .winner_text
      width: calc(100% - 86px)
      plaintext, p
        text-align: left
        margin-left: 0
        margin-right: 0
        font-size: $text-small
        color: $grey-dark
        font-family: $primary-font
        font-weight: 600

    .winner_nickname
      font-size: $text-l-medium
      margin-bottom: 6px
    .winner_follow
      margin-top: 6px
      span
        display: inline-block
        color: $grey
        strong
          color: $dark
        &:not(:last-of-type)
          margin-right: 4px
  &_images
    .image_wrapper
      position: relative
      width: calc((100vw - 34px)/3)
      height: calc((100vw - 34px)/3)
      max-width: 152px
      max-height: 152px
      margin-top: 1px
      .video
        position: absolute
        top: 5px
        right: 3px
        width: 25px
        height: 25px
        background-image: url('/images/icons/play.svg')
        background-repeat: no-repeat
        background-size: 100%
        background-position: center
      &:not(:nth-of-type(3n+3))
        margin-right: 1px
      img
        height: 100%
        width: 100%
        object-fit: cover

</style>
