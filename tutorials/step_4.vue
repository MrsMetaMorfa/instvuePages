<template>
  <div class="wrapper step_4">

    <div v-if="this.$store.state.tutorial.substage === 1">
      <img src="/images/tutorials/robot_1.png" width="100" />
      <p>
        Отлично мы прошли отборочные этапы ротации<br>
        Теперь мы <span>видим</span>, наиболее <span>интересного человека</span> из ротации.<br>
      </p>
      <button @click="nextStage" class="btn btn-default">Интересно &#129303;</button>
    </div>

  </div>
</template>

<script>

export default {
  name: 'TutorialStep4',
  data: function() {
      return {
        stages: 4
      }
  },
  beforeMount: function() {

    if (document.location.pathname !== '/rotation') {
      this.$router.push('rotation')
    }

  },
  mounted: function() {
    this.$store.state.tutorial.substage--;
    this.nextStage()
  },
  methods: {

    nextStage : function() {

      this.$store.commit('nextStage');

      switch (this.$store.state.tutorial.substage) {
        case 2:
          window.removeAllSelectedForTutorial();
          window.selectForTutorial('.winner_profile', false);
          break;

        case 3:
          window.removeAllSelectedForTutorial();
          window.selectForTutorial('.winner_view', false);
          break;

        case 4:
          window.removeAllSelectedForTutorial();
          window.selectForTutorial('.winner_buttons', false);
          break;

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
