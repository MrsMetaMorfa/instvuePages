<template lang="pug">
    div.errors-block(v-if='errors.length')
        ul
            li(v-for="error in errors") {{ error }}
</template>

<script>
export default {
    name: 'ErrorsBlock',
    props : ['errors']
}
</script>

<style scoped lang="sass">
@import "../../sass/settings.scss"
.errors-block
    width: 100%
    color: $error
    padding-left: 20px
    text-align: left
    margin: 0 0 10px
    li
        list-style-type: disc
</style>
