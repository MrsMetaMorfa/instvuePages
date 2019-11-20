<template>
  <div class="wrapper step_7">

    <div v-if="!on_tutorial_page">
      <img src="/images/tutorials/robot_1.png" width="100" />
      <p>
        Давай перейдем в раздел <span>обучения</span><br>
        Этот раздел можно найти в нижнем правом углу<br>
      </p>
    </div>

    <div v-if="on_tutorial_page && this.$store.state.tutorial.substage === 1">
      <img src="/images/tutorials/robot_1.png" width="100" />
      <p>
        Это раздел <span>обучения</span>.<br>
        В нем вы можете находить полезную информацию для развития вашего аккаунта, смежные навыки и подробное обучение нашему сервису. Этот раздел регулярно пополняется.
      </p>
      <button @click="nextStage" class="btn btn-default">Буду знать!</button>
    </div>

    <div v-if="on_tutorial_page && this.$store.state.tutorial.substage === 2">
      <img src="/images/tutorials/robot_1.png" width="100" />
      <p>
        Поздравляю &#128293; Вы прошли начально обучение.<br>
        Теперь проходите ротации, ищите интересных людей, зарабатывайте баллы, получайте статистику и рекомендации, улучшайте ваш аккаунт и наслаждайтесь нашим сервисом!
      </p>
      <button @click="nextStep" class="btn btn-default">Поехали!</button>
    </div>

  </div>
</template>

<script>

export default {
  name: 'TutorialStep8',
  data: function() {
      return {
        stages: 2,
        on_tutorial_page: false
      }
  },
  mounted: function() {
    window.removeAllSelectedForTutorial();

    if (document.location.pathname === '/guides') {
      this.on_tutorial_page = true;
    } else {
      window.removeAllSelectedForTutorial();
      window.selectForTutorial('nav', true, 1);
    }

    this.$store.state.tutorial.substage--;
    this.nextStage();

  },
  methods: {
    nextStage : function() {

      this.$store.commit('nextStage');

      if (this.$store.state.tutorial.substage > this.stages) {
        this.$store.state.tutorial.done = true;
      }

    },
    nextStep: function() {
      this.$store.commit('nextStep');
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
