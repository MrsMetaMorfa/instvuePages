<template>
  <div class="modal" @click="closeModal">
    <div class="modal-block card notification wrapper" v-if='notificationModal' @click="stopPropagation">
      <div class="notification_title container container--justifyed">
        <p class="title" v-if="showTitle"> Уведомления</p>
        <p class="choose-counter" v-if="showCounter"> Выбрано: 1</p>
        <p class="counter" v-if="showTitle">{{ not_read_count }}</p>
        <p class="delete" v-if="showCounter">
          <button class="btn delete-all" type="button"> Удалить все</button>
          <button class="btn delete-btn" type="button"> Удалить
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
              <path d="M472,83H351V60c0-33.084-26.916-60-60-60h-70c-33.084,0-60,26.916-60,60v23H40c-11.046,0-20,8.954-20,20s8.954,20,20,20h20.712l24.374,315.987c0.007,0.092,0.015,0.185,0.023,0.278c1.816,19.924,10.954,38.326,25.73,51.816C125.615,504.571,144.771,512,164.778,512h182.444c41.667,0,75.917-31.032,79.669-72.183c1.003-11.001-7.101-20.731-18.101-21.734c-11.011-1.003-20.731,7.101-21.734,18.101C385.195,456.603,368.07,472,347.222,472H164.778c-20.777,0-37.875-15.571-39.823-36.242L100.831,123h310.338l-17.082,221.462c-0.849,11.013,7.39,20.629,18.403,21.479c0.524,0.04,1.043,0.06,1.56,0.06c10.347,0,19.11-7.974,19.919-18.463L451.288,123H472c11.046,0,20-8.954,20-20S483.046,83,472,83z M311,83H201V60c0-11.028,8.972-20,20-20h70c11.028,0,20,8.972,20,20V83z" />
              <path d="M165.127,163.019c-11.035,0.482-19.59,9.818-19.108,20.854l10,228.933c0.469,10.738,9.322,19.128,19.966,19.128c0.294,0,0.591-0.006,0.888-0.02c11.035-0.482,19.59-9.818,19.108-20.854l-10-228.934C185.499,171.092,176.145,162.523,165.127,163.019z" />
              <path d="M326.019,182.127l-10,228.934c-0.482,11.035,8.073,20.372,19.108,20.854c0.297,0.013,0.593,0.02,0.888,0.02c10.643,0,19.497-8.39,19.966-19.128l10-228.933c0.482-11.035-8.073-20.372-19.108-20.854C335.856,162.527,326.501,171.092,326.019,182.127z" />
              <path d="M236,183v228.933c0,11.046,8.954,20,20,20c11.046,0,20-8.954,20-20V183c0-11.046-8.954-20-20-20S236,171.954,236,183z" />
            </svg>
          </button>
        </p>
      </div>
      <div class="notification_wrapper" v-for="item in list">

        <div class="notification_item container" @click="readItem(item.id)">
          <div class="icon" :class="getIcon(item.icon, 1)">
            <img :src="'/images/icons/notifications/' + getIcon(item.icon, 2)" alt="">
          </div>
          <p v-html="makeHtml(item.text)"></p>
          <div class="mark" :class="!item.was_read ? 'mark-new' : ''"></div>
        </div>

      </div>
      <div class="notification_all">
        <button class="btn" type="button" @click="readItem('all')">Просмотреть все</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ModalNotification",
  props: {
    notificationModal: Boolean
  },
  mounted: function() {

      this.not_read_count = Number(this.$store.state.notifications.not_read_count.replace('+',''));

      this.list = this.$store.state.notifications.notifications || [];

  },
  data: function () {
    return {
      showTitle: true,
      showCounter: false,

      /*
        su - success
        wa - warning
        er - error
        av - avatar
      */
      not_read_count : 0,
      list : []

    }
  },
  methods: {
    openNotificationModal: function () {
      this.$emit('notificationModal', false)
    },
    closeModal: function () {
      this.$emit('notificationModal', false);
      this.$root.updateNotifications();
    },
    stopPropagation: function (e) {
      e.stopPropagation()
    },
    getIcon: function (icon, part) {

        if (!icon) return '';

        let match = icon.match(/(.+)-(.+)/);

        if (!match) return icon;

        if (part != 1) return match[ part ];

        // нужно вернуть название класса
        let prefix = match[1];

        switch (prefix) {

            case "er":
                return "error";
                break;

            case "wa":
                return "warning";
                break;

            case "su":
                return "success";
                break;

            case "av":
                return "avatar";
                break;

        }



    },
    readItem: function (id) {

        if (id === 'all') {
            // прочитать всё
            this.list.forEach(e => {
                e.was_read = true;
            });

            axios({
                method : 'POST',
                url : '/api/notifications/read',
                data : {
                    all : true
                }
            })
            .then(response => {

            })
            .catch(res => {

            });

            this.not_read_count = 0;

            return;
        }

        let item = this.list.find(e => e.id === id);

        if (!item || item.was_read) return;

        item.was_read = true;

        // отправить сообщение как прочитанное
        axios({
            method : 'POST',
            url : '/api/notifications/read',
            data : {
                ids : [id]
            }
        })
        .then(response => {

        })
        .catch(res => {

            item.was_read = false;

        });

        this.not_read_count--;

    },
    makeHtml: function (text) {

        let matches = text.match(/{{[\wа-яА-Я0-9={}\/]+}}/g);

        if (!matches) matches = [];

        for (let i = 0; i < matches.length; i++) {

            let parametrs = matches[i].match(/{[\wа-яА-Я0-9=\/]+}/g);

            if (!parametrs) continue;

            let _vars = {};

            for (let j = 0; j < parametrs.length; j++) {
                let vars_array = parametrs[j].match(/([\wа-яА-Я0-9\/]+)=([\wа-яА-Я0-9\/]+)/);
                _vars[ vars_array[1] ] = vars_array[2];
            }

            let to_replace = "";

            if (_vars['link']) {
                let _a = document.createElement('a');

                _a.setAttribute('href', _vars['link']);

                _a.innerText = _vars['text'];

                to_replace = _a.outerHTML;
            }

            text = text.replace(matches[i], to_replace);
        }

        // заменим instagram ссылку
        matches = text.match(/@\S+/g);

        if (!matches) matches = [];

        for (let i = 0; i < matches.length; i++) {
            text = text.replace(matches[i], `<a href="https://www.instagram.com/${matches[i].substr(1)}" target="_blank">${matches[i]}</a>`);
        }

        return text;

    }
  }
}
</script>

<style scoped lang="sass">
@import "../../../sass/settings.scss"

.modal
  position: fixed
  z-index: 1500
  top: 0
  left: 0
  right: 0
  bottom: 0
  overflow: hidden
  display: flex
  justify-content: center
  align-items: center
  padding: 10px
  background: rgba(58,72,89,.6)
  &-block
    width: 100%
    height: auto
    margin: auto 0
    border: none
    border-radius: 3px
    background: $white
    box-shadow: 0 4px 12px 0 rgba(34, 40, 46, .1)
    padding: 16px 12px
    transition: transform 0.3s ease-in-out
  &.skew-enter, &.skew-leave-to
    transform: scale(1)
  &.skew-leave, &.skew-enter-to
    transform: scale(1)
  &.skew-enter .modal-block, &.skew-leave-to .modal-block
      transform: scale(0)
  &.skew-leave .modal-block, &.skew-enter-to .modal-block
      transform: scale(1)

.notification
  padding: 0
  overflow: hidden
  &_title
    background: $bright
    padding: 9px 15px
    color: $white
    align-items: center
    .counter
      display: block
      width: auto
      padding: 2px 4px 2px 3px
      background: $white
      border-radius: 3px
      color: $bright
      font-weight: 600
    .delete
      display: flex
      &-all
        margin-left: 8px
        font-size: $text-medium
        font-weight: 600
        font-family: $primary-font
      &-btn
        margin-left: 8px
        display: block
        width: auto
        padding: 4px
        background: $white
        border-radius: 3px
        font-size: 0
        line-height: 0
        svg
          height: 13px
          width: auto
          display: block
          path
            fill: $dark
  &_all .btn
    width: 100%
    padding: 9px 15px
    border-top: 1px solid $grey-light
    text-align: center
    font-weight: 600
    color: $bright
  &_wrapper
    overflow-y: auto
    max-height: 163px
    .notification_item
      padding: 8px 15px
      align-items: center
      font-weight: 600
      color: $grey
      a
        color: $bright
      span
        color: $dark
        font-weight: 600
      &:not(:first-of-type)
        border-top: 1px solid $grey-light
      .icon
        width: 24px
        height: 24px
        border-radius: 50%
        background: $grey
        margin-right: 8px
        overflow: hidden
        display: flex
        align-items: center
        justify-content: center
        img
          height: 14px
          width: auto
        &.avatar img
          height: 100%
          width: 100%
        &.warning
          background: $warning
        &.success
          background: $green
        &.error
          background: $error
      .mark
        width: 16px
        height: 24px
        position: relative
        margin-left: auto
        &-new:after
          content: ''
          display: block
          width: 8px
          height: 8px
          background: $bright
          border-radius: 50%
          position: absolute
          right: 0
          top: 50%
          margin-top: -4px
      &.choose
        background: $bright-lightest
        position: relative
        &:after
          content: url("/images/icons/151-check.svg")
          display: block
          width: 16px
          height: 16px
          position: absolute
          border-radius: 50%
          background: $bright
          border: solid $bright
          border-width: 1px 4px 4px 4px
          top: 50%
          margin-top: -8px
          right: 15px
</style>
