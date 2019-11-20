<template>
  <div id="tutorial_overlay" v-if="!this.$store.state.tutorial.done">
    <step_0 v-if="this.$store.state.tutorial.step === 0"></step_0>
    <step_1 v-if="this.$store.state.tutorial.step === 1"></step_1>
    <step_2 v-if="this.$store.state.tutorial.step === 2"></step_2>
    <step_3 v-if="this.$store.state.tutorial.step === 3"></step_3>
    <step_4 v-if="this.$store.state.tutorial.step === 4"></step_4>
    <step_5 v-if="this.$store.state.tutorial.step === 5"></step_5>
    <step_6 v-if="this.$store.state.tutorial.step === 6"></step_6>
    <step_7 v-if="this.$store.state.tutorial.step === 7"></step_7>
  </div>
</template>

<script>
import step_0 from './tutorials/step_0.vue'
import step_1 from './tutorials/step_1.vue'
import step_2 from './tutorials/step_2.vue'
import step_3 from './tutorials/step_3.vue'
import step_4 from './tutorials/step_4.vue'
import step_5 from './tutorials/step_5.vue'
import step_6 from './tutorials/step_6.vue'
import step_7 from './tutorials/step_7.vue'

export default {
  name: 'Tutorial',
  components: {
    step_0,
    step_1,
    step_2,
    step_3,
    step_4,
    step_5,
    step_6,
    step_7
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="sass">
@import "../../sass/settings.scss"

#tutorial_overlay
  display: flex
  align-items: center
  background-color: rgba(0,0,0,.85)
  position: fixed
  top: 0
  left: 0
  width: 100%
  height: 100%
  z-index: 99998

</style>
