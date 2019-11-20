<template lang="pug">
.wrapper.loading_wrapper
    div(:class="status")
</template>

<script>

export default {
    name: 'LoadingBlock',
    props: ['status']
}

</script>

<style scoped lang="sass">
@import "../../sass/settings.scss"

.loading_wrapper
    width: 100%
    height: 100%
    .loading
        width: 100%
        height: 100%
        background-image: url('/images/loading-rotation.svg')
        background-repeat: no-repeat
        background-size: 20%
        background-position: center

</style>
