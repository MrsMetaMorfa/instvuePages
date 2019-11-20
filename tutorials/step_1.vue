<template>
  <div class="wrapper step_1">

    <div v-if="this.$store.state.tutorial.substage === 1">
      <img src="/images/tutorials/robot_1.png" width="100" />
      <p>
        Добро пожаловать &#128293;<br>
        Я ваш персональный помощник и сейчас покажу возможности <span>GetFoll</span>!<br>
      </p>
      <button @click="nextStage" class="btn btn-default">Окей!</button>
    </div>

    <div v-if="this.$store.state.tutorial.substage === 2">
      <img src="/images/tutorials/robot_1.png" width="100" />
      <p>
        Сейчас вы находитесь на странице <span>ротации</span> <br>
      </p>
      <button @click="nextStage" class="btn btn-default">Далее</button>
    </div>

  </div>

</template>

<script>

export default {
  name: 'TutorialStep1',
  data: function() {
      return {
        stages: 2 // количество подэтапов
      }
  },
  mounted: function() {
    this.$store.state.tutorial.substage--;
    this.nextStage()
  },
  methods: {
    nextStage : function() {

      if (this.$store.state.tutorial.substage >= this.stages) {
        this.$store.commit('nextStep');
      } else {
        this.$store.commit('nextStage');
      }

      if (document.location.pathname !== '/rotation') {
        this.$router.push('rotation')
      }

    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="sass">
@import "../../../sass/settings.scss"

img
  margin: 0 auto

p
  padding: 5px
  margin-top: 40px
  text-align: center
  color: $white
  font-size: 14px
  span
    color: $bright
    font-size: 16px

button
  display: inline-block
  float: right
  margin-top: 40px
  margin-right: 10px
  width: initial
</style>
