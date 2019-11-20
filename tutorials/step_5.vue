<template>
  <div class="wrapper step_5">

    <div v-if="!on_profile_page">
      <img src="/images/tutorials/robot_1.png" width="100" />
      <p>
        Круто &#128293; Мы прошли первую ротацию<br>
        Теперь давайте перейдем на вкладку <span>Профиль</span><br>
        Она находится внизу посередине
      </p>
    </div>

    <div v-if="on_profile_page && this.$store.state.tutorial.substage === 1">
      <img src="/images/tutorials/robot_1.png" width="100" />
      <p>
        Это страница вашего профиля. Здесь вы можете отслеживать <span>статистику</span> и <span>улучшения</span> вашего аккаунта. И много другое.<br>
      </p>
      <button @click="nextStage" class="btn btn-default">Далее</button>
    </div>

    <div v-if="on_profile_page && this.$store.state.tutorial.substage === 2">
      <p>
        Это твой аватар и никнейм в <span>Instagram</span><br>
        Если кликнуть на аватар, то ты увидешь как он выглядит для пользователей в ротации<br>
      </p>
      <button @click="nextStage" class="btn btn-default">Далее</button>
    </div>

    <div v-if="on_profile_page && this.$store.state.tutorial.substage === 4">
      <img src="/images/tutorials/robot_1.png" width="100" />
      <p>
        Отлично &#128293; Вскоре мы начнем получать сообщения от других пользователей<br>
      </p>
      <button @click="nextStage" class="btn btn-default">Далее</button>
    </div>

    <div v-if="on_profile_page && this.$store.state.tutorial.substage === 5">
      <img src="/images/tutorials/robot_1.png" width="100" />
      <p>
        Пройдя ротацию в начале обучения, мы <span>заработали</span> немного энергии. <br>
        Давай посмотрим сколько у тебя её?
      </p>
      <button @click="nextStage" class="btn btn-default">Давай</button>
    </div>

  </div>
</template>

<script>

export default {
  name: 'TutorialStep6',
  data: function() {
      return {
        stages: 9,
        on_profile_page: false
      }
  },
  beforeMount: function() {

    if (document.location.pathname === '/profile') {
      this.on_profile_page = true;
      window.removeAllSelectedForTutorial();
    } else {
      window.selectForTutorial('nav', false);
    }

  },
  mounted: function() {
    this.$store.state.tutorial.substage--;
    this.nextStage()
  },
  methods: {

    nextStage : function() {
      this.$store.commit('nextStage');

      if (this.$store.state.tutorial.substage > this.stages) {
        this.$store.commit('nextStep');
      }

      if (this.$store.state.tutorial.substage === 2) {
        window.removeAllSelectedForTutorial();
        window.selectForTutorial('.profile_avatar > .wrapper', false);
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
  margin-top: -160px
</style>
