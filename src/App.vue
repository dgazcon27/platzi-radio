<template lang="pug">
    #app
        pm-header
        router-view
        pm-footer
</template>

<script>

import PmFooter from '@/component/layout/Footer.vue';
import PmHeader from '@/component/layout/Header.vue';

export default {
    name: 'app',
    components: {
        PmFooter, 
        PmHeader
    }
}
</script>

<style lang="scss">
    @import './scss/main.scss';
    .results {
        margin-top: 50px;
    }
    .is-active {
        border:3px #23d160 solid;
    }
</style>
