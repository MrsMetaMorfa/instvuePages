<template lang="pug">
.training
  .wrapper
    ul.training_list

      li
        button.btn.card.lessons-getfoll(type='button', @click="openLessons")
          .icon-wrapper(id="lessonsGetfoll")
            img(src="/images/icons/getfoll-2.png", alt="")
          span Обучение по GetFoll
          svg(xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512")
            path(d="M506.157,132.386c-7.803-7.819-20.465-7.831-28.285-0.029l-207.73,207.299c-7.799,7.798-20.486,7.797-28.299-0.015L34.128,132.357c-7.819-7.803-20.481-7.79-28.285,0.029c-7.802,7.819-7.789,20.482,0.029,28.284l207.701,207.27c11.701,11.699,27.066,17.547,42.433,17.547c15.358,0,30.719-5.846,42.405-17.533L506.128,160.67C513.946,152.868,513.959,140.205,506.157,132.386z")

</template>
<script>
export default {
  name: "GuidesPage",
  methods: {
    openLessons: function ($event) {
      let button = $event.target
      while(button.tagName !== 'BUTTON') {
        button = button.parentNode
      }
      let buttonArray = document.querySelectorAll('.training_list .btn')
      for (let i = 0; i < buttonArray.length; i++) {
        buttonArray[i].classList.remove('active')
      }
      button.classList.toggle('active')
      this.$emit('openLessons', true)
    }
  }
}
</script>

<style scoped lang="sass">
@import "../../sass/settings.scss"
.training
  &_list
    li
      display: block
      width: 100%
      margin-bottom: 10px
      .btn
        width: 100%
        display: flex
        padding: 12px 15px
        text-align: left
        color: $grey
        font-weight: 600
        transition: color 0.3s ease-in-out
        align-items: center
        .icon-wrapper
          display: block
          padding: 4px
          border-radius: 3px
          width: 19px
          height: 19px
          margin-right: 8px
          img
            height: 100%
            width: auto
          &#lessonsDecor
            background: $orangeDark
          &#lessonsPhoto
            background: $purple
          &#lessonsWrite
            background: $skyblue
        svg
          margin-left: auto
          width: 8px
          height: auto
          transform: rotate(-90deg)
          path
            fill: $grey
            transition: fill 0.3s ease-in-out
        &.active
          color: $dark
          svg path
            fill: $dark



</style>
