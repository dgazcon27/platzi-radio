<template lang="pug">
    #app
        pm-header
        pm-loader(v-show="isLoading")
        section.section(v-show="!isLoading")
            nav.nav.has-shadow
                .container
                    input.input.is-large(
                      type="text" 
                      placeholder="Buscar canciones" v-model="searchQuery"
                    )
                    a.button.is-info.is-large(@click="search") Buscar
                    a.button.is-danger.is-large &times;
            .container
                p(v-show="this.tracks.length > 0") 
                    small {{ searchMessage}}
            .container.results
                .columns.is-multiline
                    .column.is-one-quarter(v-for="tr in tracks") 
                        pm-track(:track="tr")

        pm-footer
</template>

<script>
// const tracks = [
//     {name: 'Caraluna', artist:'Bacano'},
//     {name: 'Dusk till dawn', artist:'Sia ft Zyan'},
//     {name: 'i was made for loving you', artist: 'The kiss'},
// ]

import trackService from '@/services/track';
import PmFooter from '@/component/layout/Footer.vue';
import PmHeader from '@/component/layout/Header.vue';
import PmTrack from '@/component/Track.vue';
import PmLoader from '@/component/shared/Loader.vue';
export default {
    name: 'app',
    components: {PmFooter, PmHeader, PmTrack, PmLoader},
    data () {
        return {
            searchQuery: '',
            tracks: [],
            isLoading: false

        }
    },
    methods: {
        search() {
            if (!this.searchQuery) {return}

            this.isLoading = true;

            trackService.search(this.searchQuery)
            .then((res)=> {
                this.tracks = res.tracks.items;
                this.isLoading = false;
            })
        }
    },
    computed: {
        searchMessage() {
            return `Se encontraron ${this.tracks.length} resultados.`
        }
    },
    created() {
        console.log('created...')
    }
}
</script>

<style lang="scss">
@import './scss/main.scss';

</style>
