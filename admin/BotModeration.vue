<template lang="pug">
.rotation_profiles.wrapper
  p.hint Выделите те аккаунты, которые нужно удалить

  span Категория
  select(id="categoryFilter" @change="load")
    option(value="none" selected) Все
    option(v-for="interest in interests" :value="interest.id") {{ interest.name }}

  .container.container--column
    .profile(v-for="bot in bots")

      div
        span.tag(v-for="tag in bot.topics" :class="tag.type === 1 ? 'main_tag' : ''") {{ printTag(tag.id) }}

      input.u-visually-hidden(type='checkbox', :class="bot.checked ? 'checked' : ''", name="profile", :id='bot.user.uid', @click.prevent="profileCheck(bot.user.uid)")

      label(:for="bot.user.uid")
        .profile_image
          img(:src="bot.user.avatar", :alt='bot.user.username')
        .profile_description
          h2.profile_name {{ bot.user.full_name }}
          p.profile_text(v-html="createHtmlBiography(bot.user.biography)")
        .checkmark
          svg(xmlns="http://www.w3.org/2000/svg", viewBox="0 0 11 9")
            path(fill-rule="evenodd", fill="rgb(255, 255, 255)", d="M0.555,4.133 L1.613,2.926 L5.897,6.668 L4.839,7.875 L0.555,4.133 Z")
            path(fill-rule="evenodd", fill="rgb(255, 255, 255)", d="M5.450,8.319 L4.215,7.292 L9.571,0.878 L10.806,1.905 L5.450,8.319 Z")

    button.btn.btn-default.apply(@click="selectBots") Принять
</template>

<script>
export default {
    name: 'Rotation_Step2',
    data: function () {
      return {
        bots : [],
        interests : window._initial_data.interests
      }
    },
    mounted: function() {
      this.load();
    },
    methods: {
        selectBots: function () {
          let left = [];
          let excluded = [];

          for (let i = 0; i < this.bots.length; i++) {
            if (this.bots[i].checked) {
              excluded.push(this.bots[i].user.uid);
            } else {
              left.push(this.bots[i].user.uid);
            }
          }

          axios({
              method : 'GET',
              url : '/api/moderation/candidates/select',
              params : {
                  left : left,
                  excluded : excluded
              }
          })
          .then(response => {

              this.load();

          });

        },
        load: function () {
          let category = document.getElementById('categoryFilter').value;

          if (category === 'none') category = null;

          axios({
              method : 'GET',
              url : '/api/moderation/candidates/get',
              params : {
                category : category
              }
          })
          .then(response => {
            let data = response.data;

            let _bots = [];

            for (let i = 0; i < data.candidates.length; i++) {
              _bots.push(Object.assign(data.candidates[i], {checked: false}));
            }

            this.bots = _bots;

          });

        },
        printTag: function (tag_id) {
          let tag = window._initial_data.interests.find(e => e.id == tag_id);
          return tag ? tag.name : 'no_trans';
        },
        createHtmlBiography: function (text) {
            return window.createHtmlBiography(text)
        },
        profileCheck: function (uid) {

          let bot = this.bots.find(e => e.user.uid == uid);

          if (bot.checked) {
            bot.checked = false;
          } else {
            bot.checked = true;
          }

          console.log(bot.checked);
        }
    }
}
</script>

<style scoped lang="sass">
@import "../../../sass/settings.scss"
.rotation_profiles
  position: relative
  padding-bottom: 10px
  .hint
    text-align: center
    color: $grey
    font-weight: 600
    margin: 20px 8% 16px
  .container
    flex-wrap: wrap
    max-width: 456px
    margin: 0 auto
    .profile
      width: 100%
      margin-bottom: 12px
      label
        flex-wrap: nowrap
        position: relative
        flex-direction: row
        display: flex
        padding: 12px 19px
        background: $white
        border: 1px solid $bright-lighter
        border-radius: 3px
        box-shadow: 0 4px 12px 0 rgba(34, 40, 46, 0.1)
        .profile_image
          margin: auto 19px auto 0
          width: 55px
          img
            width: 55px
            height: 55px
            border-radius: 50%
            border: 4px solid $bright-lighter
            background: $bright-lighter
            box-shadow: 0 4px 12px 0 rgba(34, 40, 46, 0.1)
        .profile_description
          width: calc(100% - 74px)
        .profile_name
          margin-bottom: 5px
        .profile_text
          font-size: $text-small
          line-height: 1.3
          color: $grey
        .checkmark
          position: absolute
          top: 50%
          right: 19px
          margin-top: -12px
          border: 3px solid $white
          border-radius: 50%
          background: $red
          width: 24px
          height: 24px
          padding: 5px 3px
          display: flex
          justify-content: center
          align-items: center
          transform: scale(0)
          transition: transform 0.3s ease-in-out
          svg
            width: auto
            height: 100%
      input.checked + label
        animation: itemTap 0.3s ease-in
        border-color: $red
        transition: border-color 0.3s ease-in
        .checkmark
          animation: itemTapIcon 0.5s ease-out forwards

.tag
  font-size: 11px
  padding: 4px
  margin: 5px
  background: white
  color: black

.main_tag
  background: #46d1bf

#categoryFilter
  margin: 15px
  padding: 5px 10px

.apply
  margin: 20px 0
</style>
