<template>
  <div class="wrapper step_3">

  </div>
</template>

<script>

export default {
  name: 'TutorialStep3',
  data: function() {
      return {
        stages: 1
      }
  },
  beforeMount: function() {

    if (document.location.pathname !== '/rotation') {
      this.$router.push('rotation')
    }

  },
  beforeDestroy: function() {

  },
  mounted: function() {
    this.$store.state.tutorial.substage--;
    this.nextStage()
  },
  methods: {

    nextStage : function() {

      this.$store.commit('nextStage');

      switch (this.$store.state.tutorial.substage) {
        case 1:
          window.removeAllSelectedForTutorial();
          window.selectForTutorial('.rotation_profiles .profile', true);
          setTimeout(function() { window.selectForTutorial('main .hint', false) }, 500);

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
  max-width: 100px
</style>
