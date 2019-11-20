<template lang="pug">
.settings.wrapper
  h2.settings_title Настройки профиля
  ul.settings_profile.card
    li.settings_item(v-for="setting in profile_settings", :data-code="setting.code")

      .tutorial_helper(v-if="showTutorialHelper(setting.code)")
        p(v-html="showTutorialHtml(setting.code)")
        button.later(@click="nextStage" class="btn btn-default") Далее

      button.btn.settings_button.container(type="button", @click="openSetting")
        .icon-wrapper(:id="setting.style_id")
          img(:src="setting.img", alt="")
        span {{ setting.title }}
        svg(xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512")
          path(d="M506.157,132.386c-7.803-7.819-20.465-7.831-28.285-0.029l-207.73,207.299c-7.799,7.798-20.486,7.797-28.299-0.015L34.128,132.357c-7.819-7.803-20.481-7.79-28.285,0.029c-7.802,7.819-7.789,20.482,0.029,28.284l207.701,207.27c11.701,11.699,27.066,17.547,42.433,17.547c15.358,0,30.719-5.846,42.405-17.533L506.128,160.67C513.946,152.868,513.959,140.205,506.157,132.386z")

      .settings_form
        ul.results-list.container
          li(v-for="tag in setting.selected_tags") {{ tag_list[setting.tags_list_name][tag].title }}
            button.btn.remove(type='button', @click="setChanged($event, tag, setting.code)", title="Remove Item")
              svg(xmlns="http://www.w3.org/2000/svg", viewBox="0 0 512.001 512.001")
                path(d="M284.286,256.002L506.143,34.144c7.811-7.811,7.811-20.475,0-28.285c-7.811-7.81-20.475-7.811-28.285,0L256,227.717L34.143,5.859c-7.811-7.811-20.475-7.811-28.285,0c-7.81,7.811-7.811,20.475,0,28.285l221.857,221.857L5.858,477.859c-7.811,7.811-7.811,20.475,0,28.285c3.905,3.905,9.024,5.857,14.143,5.857c5.119,0,10.237-1.952,14.143-5.857L256,284.287l221.857,221.857c3.905,3.905,9.024,5.857,14.143,5.857s10.237-1.952,14.143-5.857c7.811-7.811,7.811-20.475,0-28.285L284.286,256.002z")

        ul.results-list.setting_error
          li(v-for="error in setting.errors") {{ error }}

        .form_list
          input(type="text", placeholder='Поиск..', @keyup="filterTags")
          ul.form_datalist
            li(v-for="tag in getTagsArray(tag_list[setting.tags_list_name])", :class="setting.selected_tags.indexOf(tag.id) > -1 ? 'changed' : ''")
              button.btn(type="button", :data-title="tag.title", @click="setChanged($event, tag.id, setting.code)")
                span {{ tag.title }}
                span.marker


  h2.settings_title Данные профиля

  .tutorial_helper(v-if="this.$store.state.tutorial.substage === 7")
    p Так же здесь можно привязать аккаунт Instagram, почту и изменить пароль
    button.later(@click="nextStep" class="btn btn-default") Понятно

  ul.settings_data.card
    li.settings_item.settings_instagram
      .settings_button.container(:class="$store.state.user.is_linked_instagram ? '' : 'disabled'")
        .icon-wrapper(id="settings_instagram")
          img(src="/images/icons/011-instagram.svg", alt="")
        span {{ $store.state.user.is_linked_instagram ? $store.state.user.instagram.username : 'Instagram не привязан' }}
        a.btn(v-if="!$store.state.user.is_linked_instagram" href="/auth/instagram" type="button") Привязать
    li.settings_item
      .settings_button.container(type="button", @click="openSetting")
        .icon-wrapper(id="settings_email")
          img(src="/images/icons/159-email-light.svg", alt="")
        span {{ $store.state.user.email ? $store.state.user.email : 'Email не привязан' }}
        button.btn(v-if="!$store.state.user.email"  type="button" @click="openEmailLinkModal") Привязать

    li.settings_item(v-if="$store.state.user.email")
      button.btn.settings_button.container(type="button", @click="openSetting")
        .icon-wrapper(id="settings_password")
          img(src="/images/icons/168-padlock-light.svg", alt="")
        span Пароль
        svg(xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512")
          path(d="M506.157,132.386c-7.803-7.819-20.465-7.831-28.285-0.029l-207.73,207.299c-7.799,7.798-20.486,7.797-28.299-0.015L34.128,132.357c-7.819-7.803-20.481-7.79-28.285,0.029c-7.802,7.819-7.789,20.482,0.029,28.284l207.701,207.27c11.701,11.699,27.066,17.547,42.433,17.547c15.358,0,30.719-5.846,42.405-17.533L506.128,160.67C513.946,152.868,513.959,140.205,506.157,132.386z")
      .settings_form
        transition(name="fade", mode="out-in")
          form.form(id='passwordChange')
            ErrorsBlock(:errors="errors")
            label
              span Cтарый пароль
              input(type="password", name="profileOldPassword", id="profileOldPassword", placeholder='Cтарый пароль')
            label
              span Новый пароль
              input(type="password", name="profileNewPassword", id="profileNewPassword", placeholder='Новый пароль')
            label
              span Подтвердить пароль
              input(type="password", name="profileRepeatPassword", id="profileRepeatPassword", placeholder='Подтвердить пароль')
            button.btn.btn-default(type='submit', @click.prevent="changePassword") Сменить пароль

</template>

<script>
import ErrorsBlock from './ErrorsBlock.vue'

export default {
  name: "Setting",
  components: {
      ErrorsBlock
  },
  mounted: function () {
      // подгрузим текущие настройки
      let _tags = {};
      let store_settings = this.$store.state.settings;

      this.tag_list = {
          interests: {},
          langs: {}
      };

      for (let i = 0; i < store_settings.info_interests.length; i++) {
          _tags[store_settings.info_interests[i].id] = {
              id : store_settings.info_interests[i].id,
              title : store_settings.info_interests[i].text
          };
      }

      this.tag_list.interests = _tags;

      _tags = {};

      for (let i = 0; i < store_settings.info_languages.length; i++) {
          _tags[store_settings.info_languages[i].id] = {
              id : store_settings.info_languages[i].id,
              title : store_settings.info_languages[i].name
          };
      }

      this.tag_list.langs = _tags;

      for (let i = 0; i < this.profile_settings.length; i++) {
          this.profile_settings[i].selected_tags = store_settings[ this.profile_settings[i].code ] || [];
      }

  },
  data: function () {
    return {

      profile_settings : [
          {
              style_id: 'settings_occupation',
              code: 'account_professions',
              title: 'Род деятельности',
              api_link: '/api/settings/profession/edit',
              api_arg: 'interest_id',
              img: '/images/icons/032-briefcase.svg',
              tags_list_name: 'interests',
              selected_tags: [],
              max: 1,
              errors : []
          },
          {
              style_id: 'settings_hobby',
              code: 'account_hobbies',
              title: 'Хобби',
              api_link: '/api/settings/hobby/edit',
              api_arg: 'interests',
              img: '/images/icons/003-diamond.svg',
              tags_list_name: 'interests',
              selected_tags: [],
              max: 5,
              errors : []
          },
          {
              style_id: 'settings_interests',
              code: 'read_interests',
              title: 'Интересы',
              api_link: '/api/settings/interest/edit',
              api_arg: 'interests',
              img: '/images/icons/029-star.svg',
              tags_list_name: 'interests',
              selected_tags: [],
              max: 5,
              errors : []
          },
          {
              style_id: 'settings_languages',
              code: 'account_languages',
              title: 'Языки аккаунта',
              api_link: '/api/settings/account_lang/edit',
              api_arg: 'langs',
              img: '/images/icons/047-earth-grid.svg',
              tags_list_name: 'langs',
              selected_tags: [],
              max: 5,
              errors : []
          },
          {
              style_id: 'settings_readLanguages',
              code: 'read_languages',
              title: 'Языки для чтения',
              api_link: '/api/settings/read_lang/edit',
              api_arg: 'langs',
              img: '/images/icons/090-binoculars.svg',
              tags_list_name: 'langs',
              selected_tags: [],
              max: 5,
              errors : []
          },
      ],

      tag_list: {
          interests: {},
          langs: {}
      },

      errors: []

    }
  },
  methods: {
    showTutorialHelper: function (code) {

      let shows = {
        'account_professions' : 2,
        'account_hobbies' : 3,
        'read_interests' : 4,
        'account_languages' : 5,
        'read_languages' : 6
      };

      if (!this.$store.state.tutorial.done && this.$store.state.tutorial.substage === 7) {
        window.selectForTutorial('.settings_data', false);
        return false;
      }

      if (!this.$store.state.tutorial.done && shows[code] === this.$store.state.tutorial.substage) {
        window.selectForTutorial('[data-code="'+code+'"]', false);
        setTimeout(function() { document.querySelector('[data-code="'+code+'"]').scrollIntoView({block: "center"}); }, 500);
        return true;
      } else {
        return false;
      }

    },
    showTutorialHtml: function (code) {

      let shows = {
        'account_professions' : '<span>Род деятельности</span> – это основная тематика вашего Instagram аккаунта. Выбрать можно только 1 род деятельности.',
        'account_hobbies' : '<span>Хобби</span> – это дополнительные тематики вашего Instagram аккаунта. Чем вы увлекаетесь и так же делитель в Instagram.',
        'read_interests' : '<span>Интересы</span> – по интересам мы подбираем интересных для вас людей, в ваших ротациях.<br>Выбирать можно любые интересы, они не обязательно должны фигурировать в вашем Instagram',
        'account_languages' : '<span>Языки аккаунта</span> – На каких языках вы ведете свой Instagram?',
        'read_languages' : '<span>Языки для чтения</span> – На каких языках вы хотите видеть аккаунты в ротации?'
      };

      return shows[code];
    },
    nextStage: function() {
      window.removeAllSelectedForTutorial();
      this.$store.commit('nextStage');
    },
    nextStep: function() {
      window.removeAllSelectedForTutorial();
      this.$store.commit('nextStep');
    },
    getTagsArray: function (tags_obj) {
        let tags_array = [];

        for (let item_id in tags_obj) {
            tags_array.push(tags_obj[item_id]);
        }

        tags_array.sort(function (a, b) {
            if (a.title > b.title) {
                return 1;
            }
            if (a.title < b.title) {
                return -1;
            }
            return 0;
        });

        return tags_array;

    },
    filterTags: function ($event) {
        // элементы li
        let listOfElements = $event.target.nextElementSibling.childNodes;
        let input_text = $event.target.value.toLowerCase().trim();

        for (let i = 0; i < listOfElements.length; i++) {
            let text = listOfElements[i].innerText.toLowerCase().trim();

            if (text.indexOf(input_text) > -1) {
                listOfElements[i].style.display = 'list-item';
            } else {
                listOfElements[i].style.display = 'none';
            }
        }

    },
    showErrors : function (errors) {
        if (typeof errors === 'string') {
            errors = [errors];
        }

        this.errors = errors;
    },
    openSetting: function ($event) {
      let button = $event.target

      while (button.tagName !== 'BUTTON') {
        button = button.parentElement
      }

      if (!button.parentNode.classList.contains('active')) {

        let settingsItems = button.parentNode.parentNode.childNodes

        for (let i = 0; i < settingsItems.length; i++) {
          settingsItems[i].classList.remove('active')
        }

        button.parentNode.classList.add('active')

      } else {

        button.parentNode.classList.remove('active')

      }

    },
    setChanged: function ($event, tag_id, setting_code) {

      let profile_setting = this.profile_settings.find(e => e.code === setting_code);

      // убираем ошибки
      profile_setting.errors = [];

      if (profile_setting.selected_tags.indexOf(Number(tag_id)) > -1) {
          // удалить тэг
          profile_setting.selected_tags.splice(profile_setting.selected_tags.indexOf(Number(tag_id)), 1);

          axios({
              method : 'POST',
              url : profile_setting.api_link,
              data : {
                  type : 'remove',
                  [profile_setting.api_arg] : [ tag_id ]
              }
          })
          .catch(res => {
              // откатим изменения
              profile_setting.selected_tags.push(Number(tag_id));

          });

      } else {
          // добавить тэг
          if (profile_setting.selected_tags.length + 1 > profile_setting.max) {
               return profile_setting.errors = ["Максимум можно выбрать " + profile_setting.max + " тэг(-ов)"];
          }

          profile_setting.selected_tags.push(Number(tag_id));

          axios({
              method : 'POST',
              url : profile_setting.api_link,
              data : {
                  type : 'add',
                  [profile_setting.api_arg] : [ tag_id ]
              }
          })
          .catch(res => {
              // откатим изменения
              profile_setting.selected_tags.splice(profile_setting.selected_tags.indexOf(Number(tag_id)), 1);
          });

      }

    },
    openEmailLinkModal: function () {
        this.$emit('openEmailLinkModal');
    },
    // отправить запрос на смену пароля
    changePassword: function ($event) {

        let self = this;

        let button = $event.target;

        if (button.classList.contains('loading')) return;

        self.showErrors([]);

        let old_password = document.getElementById('profileOldPassword').value.trim();
        let new_password = document.getElementById('profileNewPassword').value.trim();
        let new_password_confirmation = document.getElementById('profileRepeatPassword').value.trim();

        if (!old_password.length || !new_password.length || !new_password_confirmation.length) {
            return self.showErrors([
                "Все поля должны быть заполнены"
            ]);
        }

        if (new_password.length > 32 || new_password.length < 6) {
            return self.showErrors([
                "Пароль должен состоять минимум из 6 символов",
                "Пароль должен состоять максимум из 32 символов"
            ]);
        }

        let match = new_password.match(this.$store.state.password_regex);

        if (!match || match[0].length !== new_password.length) {
            return self.showErrors([
                "Пароль должен содержать только буквы латинского алфавита и цифры"
            ]);
        }

        if (new_password !== new_password_confirmation) {
            return self.showErrors([
                "Пароли должны совпадать"
            ]);
        }

        axios({
            method : 'POST',
            url : '/api/settings/password/change',
            data : {
                old_password : old_password,
                new_password: new_password,
                new_password_confirmation : new_password_confirmation
            }
        })
        .then(response => {

            document.getElementById('profileOldPassword').value = '';
            document.getElementById('profileNewPassword').value = '';
            document.getElementById('profileRepeatPassword').value = '';

            iziToast.custom.success({
                message : response.data.message
            });

            button.classList.remove('loading');
            button.innerHTML = 'Сменить пароль';

        })
        .catch(res => {
            self.showErrors(res.response.data.message);
            button.classList.remove('loading');
            button.innerHTML = 'Сменить пароль';
        });


        button.classList.add('loading');
    }
  }
}
</script>

<style scoped lang="sass">
@import "../../sass/settings.scss"

.container
    display: flex !important
.settings
  &_title
    color: $grey
    font-weight: 600
    margin: 0 0 8px
    font-size: $text-small
  &_profile, &_data
    overflow: hidden
  &_profile
    margin-bottom: 20px
  &_item
    &:not(:last-of-type)
      border-bottom: 1px solid $grey-lighter
    .settings_button
      flex-wrap: nowrap
      width: 100%
      padding: 12px 15px
      align-items: center
      font-size: $text-medium
      transition: background 0.3s ease-in-out
      .icon-wrapper
        border-radius: 3px
        width: 19px
        height: 19px
        display: flex
        justify-content: center
        align-items: center
        margin-right: 10px
        background: $disabled
        padding: 3px
        &#settings_occupation
          background: $orange
        &#settings_hobby
          background: $blue
        &#settings_interests
          background: $purple
        &#settings_languages
          background: $green
        &#settings_readLanguages
          background: $red
        &#settings_instagram
          background: $purple
        &#settings_email
          background: $lightblue
        &#settings_password
          background: $limegreen
      span
        color: $grey
        font-weight: 600
        transition: color 0.3s ease-in-out
        font-size: $text-medium
      svg
        margin-left: auto
        width: 8px
        height: auto
        transition: transform 0.3s ease-in-out
        path
          fill: $grey
          transition: fill 0.3s ease-in-out
      a, button
        color: $bright
        margin-left: auto
        text-decoration: underline
        font-weight: 600
        transition: text-decoration 0.2s ease-in-out
        &:hover, &:focus
          text-decoration: none
      &.disabled
        .icon-wrapper
          background: $disabled!important
        span
          color: $disabled
    .settings_form
      background: $grey-lighter
      padding: 0 15px
      color: $grey
      transition: padding 0.3s ease-in-out
      .form
        label
          width: 100%
          display: block
          margin-bottom: 0
          height: 0
          overflow: hidden
          position: relative
          transition: margin 0.3s ease-in-out, height 0.3s ease-in-out
          span
            font-size: 0
            line-height: 0
            position: absolute
            visibility: hidden
          input
            width: 100%
            background: $white
            border-radius: 3px
            border: 1px solid transparent
            padding: 10px 13px 10px 34px
            &:hover, &:focus
              border-color: $bright
          &.success
            &:before
              content: ''
              display: flex
              justify-content: center
              align-items: center
              border-radius: 50%
              width: 13px
              height: 13px
              background: $bright
              padding: 3px
              position: absolute
              top: 50%
              margin-top: -6.5px
              right: 11px
              z-index: 10
            &:after
              content: url("/images/icons/151-check.svg")
              position: absolute
              width: 7px
              height: 7px
              z-index: 11
              top: 50%
              margin-top: -8px
              right: 14px
          &.error
            &:before
              content: ''
              display: flex
              justify-content: center
              align-items: center
              border-radius: 50%
              width: 13px
              height: 13px
              background: $error
              padding: 3px
              position: absolute
              top: 50%
              margin-top: -6.5px
              right: 11px
              z-index: 10
            &:after
              content: url("/images/icons/034-cancel.svg")
              position: absolute
              width: 6px
              height: 6px
              z-index: 11
              top: 50%
              margin-top: -8px
              right: 14px
        button
          font-weight: bold
          font-size: $text-medium
          height: 0
          padding-top: 0
          padding-bottom: 0
          border-width: 0 1px 0 1px
          overflow: hidden
          transition: padding 0.3s ease-in-out, height 0.3s ease-in-out, border 0.3s ease-in-out
        &#emailChange
          input
            background-image: url('/images/icons/159-email.svg')
            background-repeat: no-repeat
            background-position: 11px center!important
            background-size: 13px auto
            &:hover, &:focus
              background-image: url('/images/icons/159-email-dark.svg')
        &#passwordChange
          input
            background-image: url('/images/icons/168-padlock.svg')
            background-repeat: no-repeat
            background-position: 11px center!important
            background-size: 13px auto
            &:hover, &:focus
              background-image: url('/images/icons/168-padlock-dark.svg')
        &#passwordChangeCode
          input
            background-image: url('/images/icons/035-shield.svg')
            background-repeat: no-repeat
            background-position: 11px center!important
            background-size: 13px auto
            &:hover, &:focus
              background-image: url('/images/icons/035-shield-dark.svg')
      .results-list
        margin-bottom: 0
        transition: margin 0.3s ease-in-out
        li
          margin: 0 6px 0 0
          border-radius: 3px
          background: $white
          padding: 0 0 0 4px
          font-size: $text-x-small
          font-weight: 600
          display: flex
          align-items: center
          overflow: hidden
          height: 0
          transition: padding 0.3s ease-in-out, margin 0.3s ease-in-out, height 0.3s ease-in-out
          button.remove
            height: 12px
            padding: 4px
            display: flex
            justify-content: center
            align-items: center
            width: auto
            svg
              height: 100%
              width: auto
              path
                fill: $grey
      .form_list
        border-radius: 3px
        background: $white
        overflow: hidden
        height: 0
        transition: height 0.3s ease-in-out
        input
          background: $white url("/images/icons/arrow-down.svg") no-repeat
          background-position: calc(100% - 13px) center
          background-size: 8px 6px
          border-bottom: 1px solid $grey-lighter
          padding: 10px 26px 10px 13px
          &:hover, &:focus
            border-bottom-color: $bright
        .form_datalist
          background: white
          overflow-y: auto
          height: 108px
          li
            width: 100%
            border-top: 1px solid transparent
            transition: background 0.2s ease-in-out
            &:not(:first-child)
              border-top-color: $grey-lighter
            button
              padding: 10px 13px
              display: flex
              align-items: center
              justify-content: space-between
              width: 100%
              .marker
                display: block
                border-radius: 50%
                width: 14px
                height: 14px
                position: relative
                background-color: $grey-lighter
                &:after
                  content: ''
                  display: block
                  position: absolute
                  border-radius: 50%
                  width: 6px
                  height: 6px
                  top: 50%
                  left: 50%
                  margin-top: -3px
                  margin-left: -3px
                  background: transparent
                  transition: background 0.2s ease-in-out
            &.changed
              background: $bright-lightest
              button
                .marker:after
                  background: $bright

    &.active
      .settings_button
        background: $grey-lighter
        span
          color: $dark
        svg
          transform: rotate(-180deg)
          path
            fill: $dark
      .settings_form
        padding-bottom: 12px
        .form
          label
            margin-bottom: 11px
            height: 37px
          button
            height: 33px
            padding-top: 9px
            padding-bottom: 8px
            border-width: 1px 1px 1px 1px
        .results-list
          margin-bottom: 4px
          li
            padding-top: 4px
            padding-bottom: 4px
            margin-bottom: 6px
            height: 20px
        .form_list
          height: 145px

.setting_error
    li
        font-size: 1em !important
        background: none !important
        color: $red
.settings_item
  .settings_button
    background-color: $white

.tutorial_helper
  font-size: 1em
  button
    width: initial
    font-size: 12px
    margin: 7px 0
</style>
