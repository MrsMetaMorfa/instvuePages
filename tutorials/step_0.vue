<template>
  <div class="wrapper step_0">

    <div v-if="!on_settings_page">
      <p v-if="on_profile_page">
        У вас не привязан инстаграм<br>
        Нажми на иконку <span>настроек</span> в верхнем левом углу
      </p>
      <p v-if="!on_profile_page">
        Нажми на кнопку <span>профиля</span> внизу
      </p>
    </div>

    <div v-if="on_settings_page && this.$store.state.tutorial.substage === 1">
      <img src="/images/tutorials/robot_1.png" width="100" />
      <p>
        Чтобы получить полный доступ к функциям <span>GetFoll</span>, тебе нужно привязать твой <span>Instagram</span> аккаунт. Сделать это просто. <br>
        Нажми на кнопку "привязать"<br>
      </p>
      <button @click="nextStage" class="btn btn-default">Далее</button>
    </div>

    <div v-if="on_settings_page && this.$store.state.tutorial.substage === 2">
      <p>
        Нажми на кнопку "привязать"<br>
      </p>
    </div>

  </div>
</template>

<script>

export default {
  name: 'TutorialStep0',
  data: function() {
      return {
        stages: 2,
        on_profile_page: false,
        on_settings_page: false,
        selector_timeout: null
      }
  },
  mounted: function() {

    if (this.$store.state.user.is_linked_instagram) {
      this.$store.commit('nextStep');
      return;
    }

    if (document.location.pathname === '/settings') {

      this.on_profile_page = false;
      this.on_settings_page = true;
      document.getElementsByTagName('nav')[0].classList.remove('selected_for_tutorial');
      document.getElementsByTagName('header')[0].classList.remove('selected_for_tutorial');

      this.$store.state.tutorial.substage--;
      this.nextStage();

    } else if (document.location.pathname === '/profile') {

      this.on_profile_page = true;
      this.on_settings_page = false;
      document.getElementsByTagName('nav')[0].classList.remove('selected_for_tutorial');
      document.getElementsByTagName('header')[0].classList.add('selected_for_tutorial');

    } else {

      this.on_profile_page = false;
      this.on_settings_page = false;
      document.getElementsByTagName('nav')[0].classList.add('selected_for_tutorial');
      document.getElementsByTagName('header')[0].classList.remove('selected_for_tutorial');

    }

  },
  methods: {
    selectForTutorial: function(selector, is_all) {

      this.selector_timeout = setTimeout(() => {

        if (is_all) {

          let elements = document.querySelectorAll(selector);

          if (!elements.length) {
            return this.selectForTutorial(selector, is_all);
          }

          for (let i = 0; i < elements.length; i++) {
            elements[i].classList.add('selected_for_tutorial');
          }


        } else {

          let element = document.querySelector(selector);

          if (!element) {
            return this.selectForTutorial(selector, is_all);
          }

          element.classList.add('selected_for_tutorial');

        }

      }, 300);

    },
    nextStage : function() {

      this.$store.commit('nextStage');

      switch (this.$store.state.tutorial.substage) {
        case 2:
          this.removeAllSelectedForTutorial();
          this.selectForTutorial('.settings_instagram', false);
          break;

        default:
          this.removeAllSelectedForTutorial();
          break;
      }

      if (this.$store.state.tutorial.substage > this.stages) {
        this.$store.commit('nextStep');
      }


    },
    removeAllSelectedForTutorial: function() {
      let elements = document.getElementsByClassName('selected_for_tutorial');

      while (elements.length) {
        elements[0].classList.remove('selected_for_tutorial')
      }

      return;
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
