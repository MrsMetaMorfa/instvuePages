<template lang="pug">
.lessons
  .wrapper
    button.btn.card.btn-back(type="button", @click="openCategories", title="К списку категорий")
      svg(xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512")
        path(d="M245,1l4.3,2.1A20,20,0,0,1,256.4,33c-1.5,1.9-3.4,3.7-5.1,5.5L50.4,239.8c-12,12.1-12,22.3,0,34.4L251.3,475.5c1.6,1.7,3.3,3.3,4.8,5.1a20.1,20.1,0,0,1-6.8,30.3L245,513h-9c-5-3.7-10.5-6.9-14.8-11.3Q120.9,401.7,20.9,301.3C.8,281.2-4.2,254.4,8,230.4a71.2,71.2,0,0,1,12.6-17.2Q122.3,110.9,224.3,9.1c3.3-3.3,7.8-5.4,11.7-8.1Z")
      span Назад

    ul.lessons_list
      li.card
        h2.lesson-title
          button.btn(type='button', @click="openLessons")
            span Стартовое интерактивное обучение
            svg(xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512")
              path(d="M506.157,132.386c-7.803-7.819-20.465-7.831-28.285-0.029l-207.73,207.299c-7.799,7.798-20.486,7.797-28.299-0.015L34.128,132.357c-7.819-7.803-20.481-7.79-28.285,0.029c-7.802,7.819-7.789,20.482,0.029,28.284l207.701,207.27c11.701,11.699,27.066,17.547,42.433,17.547c15.358,0,30.719-5.846,42.405-17.533L506.128,160.67C513.946,152.868,513.959,140.205,506.157,132.386z")
        .lesson_wrapper
          p Пройдите интерактивное обучение по функционалу сайта.
          p Каждый этап обучения расскажет вам что-то интересное
          .lesson_video
            a(href="/tutorial/start")
              button.btn(type="button", title='Начать обучение') Запуск

</template>
<script>
export default {
  name: "GuidesDescriptions",
  data: function () {
    return {
      lesson1: {
        videoVisible: false,
        videoPictureVisible: true
      },
      lesson2: {
        videoVisible: false,
        videoPictureVisible: true
      },
      lesson3: {
        videoVisible: false,
        videoPictureVisible: true
      },
      lesson4: {
        videoVisible: false,
        videoPictureVisible: true
      }
    }
  },
  methods: {
    openLessons: function ($event) {
      let button = $event.target
      while (button.tagName !== 'BUTTON') {
        button = button.parentNode
      }
      if (!button.parentNode.classList.contains('active')) {
        let buttonArray = document.querySelectorAll('.lesson-title')
        for (let i = 0; i < buttonArray.length; i++) {
          buttonArray[i].classList.remove('active')
        }
        button.parentNode.classList.add('active')
      } else {
        button.parentNode.classList.remove('active')
      }
    },
    startVideo: function ($event) {
      let button = $event.target
      while (button.tagName !== 'BUTTON') {
        button = button.parentNode
      }
      let lessonTarget = button.getAttribute('data-target')
      button.remove()
      // button.parentNode.removeChild(button)
      console.log(lessonTarget)
      switch (lessonTarget) {
        case 'lesson1':
          this.lesson1 = {
            videoVisible: true,
            videoPictureVisible: false
          }
          break
        case 'lesson2':
          this.lesson2 = {
            videoVisible: true,
            videoPictureVisible: false
          }
          break
        case 'lesson3':
          this.lesson3 = {
            videoVisible: true,
            videoPictureVisible: false
          }
          break
        case 'lesson4':
          this.lesson4 = {
            videoVisible: true,
            videoPictureVisible: false
          }
          break
      }
    },
    openCategories: function () {
      this.$emit('openLessons', false)
    }
  }
}
</script>

<style scoped lang="sass">
@import "../../sass/settings.scss"
.lessons
  .btn-back
    margin-bottom: 10px
    width: auto
    padding: 6px 12px
    display: flex
    color: $grey
    align-items: center
    font-size: $text-small
    svg
      height: 9px
      width: auto
      margin-right: 4px
      path
        fill: $grey
    &:hover, &:focus
      color: $dark
      svg path
        fill: $dark
  &_list
    li
      display: block
      width: 100%
      margin-bottom: 10px
      .lesson-title
        .btn
          width: 100%
          display: flex
          padding: 12px 15px
          text-align: left
          color: $grey
          font-weight: 600
          transition: color 0.3s ease-in-out
          align-items: center
          svg
            margin-left: auto
            width: 8px
            height: auto
            transition: transform 0.3s ease-in-out
            path
              fill: $grey
              transition: fill 0.3s ease-in-out
        &.active .btn
          color: $dark
          svg
            transform: rotate(-180deg)
            path
              fill: $dark
      .lesson_wrapper
        padding: 0 15px
        height: 0
        overflow: hidden
        p
          color: $grey
          margin-bottom: 15px
        .lesson_video
          width: 100%
          height: auto
          position: relative
          border-radius: 3px
          overflow: hidden
          img, iframe
            width: 100%
            height: auto
            display: block
            margin: 0
          .btn-video
            height: 44px
            width: 62px
            border: none
            background: none
            position: absolute
            top: 50%
            margin-top: -22px
            left: 50%
            margin-left: -31px
            padding: 0
            z-index: 10
            img
              width: 100%
              height: 100%
      .lesson-title.active
          & + .lesson_wrapper
            height: auto
            padding-bottom: 12px
</style>
