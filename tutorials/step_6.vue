<template>
  <div class="wrapper step_6">

    <div v-if="!on_settings_page">
      <img src="/images/tutorials/robot_1.png" width="100" />
      <p v-if="on_profile_page">
        Что бы вы начали показываться в ротации, <span>необходимо настроить ваш профиль</span>.
        Для этого перейдите в раздел «<span>Настройки</span>», в верхнем левом углу
      </p>
      <p v-if="!on_profile_page">
        Нажми на кнопку <span>профиля</span> внизу
      </p>
    </div>

    <div v-if="on_settings_page && this.$store.state.tutorial.substage === 1">
      <img src="/images/tutorials/robot_1.png" width="100" />
      <p>
        Это важный раздел!<br>
        На основе настроек, идет <span>подбор пользователей</span>, которые <span>увидят вас</span> и которых <span>увидите вы</span>.<br>
      </p>
      <button @click="nextStage" class="btn btn-default">Понятно</button>
    </div>

  </div>
</template>

<script>

export default {
  name: 'TutorialStep7',
  data: function() {
      return {
        stages: 12,
        on_profile_page: false,
        on_settings_page: false
      }
  },
  mounted: function() {

    if (document.location.pathname === '/settings') {

      this.on_profile_page = false;
      this.on_settings_page = true;
      window.removeAllSelectedForTutorial();

      this.$store.state.tutorial.substage--;
      this.nextStage();

    } else if (document.location.pathname === '/profile') {

      this.on_profile_page = true;
      this.on_settings_page = false;
      window.removeAllSelectedForTutorial();
      window.selectForTutorial('header', true, 1);

    } else {

      this.on_profile_page = false;
      this.on_settings_page = false;
      window.removeAllSelectedForTutorial();
      window.selectForTutorial('nav', true, 1);

    }

  },
  methods: {

    nextStage : function() {

      this.$store.commit('nextStage');

      if (this.$store.state.tutorial.substage >= this.stages) {
        this.$store.commit('nextStep');
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

.top_middle
  margin-top: -200px

.bottom_middle
  margin-top: 160px
</style>
