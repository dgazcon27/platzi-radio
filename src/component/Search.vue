<template lang="pug">
    main
        pm-notification(v-show="showNotification")
            p(slot="body") No se encontraron resultados
        pm-loader(v-show="isLoading")
        section.section(v-show="!isLoading")
            nav.nav
                .container
                    input.input.is-large(
                      type="text" 
                      placeholder="Buscar canciones" v-model="searchQuery",
                      @keyup.enter="search"
                    )
                    a.button.is-info.is-large(@click="search") Buscar
                    a.button.is-danger.is-large &times;
            .container
                p(v-show="this.tracks.length > 0") 
                    small {{ searchMessage}}
            .container.results
                .columns.is-multiline
                    .column.is-one-quarter(v-for="tr in tracks") 
                        pm-track(
                        v-blur="tr.preview_url"
                        :track="tr", 
                        :class="{'is-active':tr.id === selectedTrack}" 
                        @select="setSelectedTrack")
</template>

<script>
// const tracks = [
//     {name: 'Caraluna', artist:'Bacano'},
//     {name: 'Dusk till dawn', artist:'Sia ft Zyan'},
//     {name: 'i was made for loving you', artist: 'The kiss'},
// ]

import trackService from '@/services/track';
import PmTrack from '@/component/Track.vue';

import PmLoader from '@/component/shared/Loader.vue';
import PmNotification from '@/component/shared/Notification.vue';

export default {
    name: 'app',
    components: {
        PmTrack, 
        PmLoader, 
        PmNotification
    },
    data () {
        return {
            searchQuery: '',
            tracks: [],
            isLoading: false,
            selectedTrack: '',
            showNotification: false

        }
    },
    methods: {
        search() {
            if (!this.searchQuery) {return}

            this.isLoading = true;

            trackService.search(this.searchQuery)
            .then((res)=> {
                this.showNotification = res.tracks.total === 0;
                this.tracks = res.tracks.items;
                this.isLoading = false;
            })
        },
        setSelectedTrack(id) {
            this.selectedTrack = id;
        }
    },
    computed: {
        searchMessage() {
            return `Se encontraron ${this.tracks.length} resultados.`
        }
    },
    watch: {
        showNotification() {
            if (this.showNotification) {
                setTimeout(() => {
                    this.showNotification = false
                }, 3000)
            }
        }
    }
}
</script>

<style lang="scss">
    .results {
        margin-top: 50px;
    }
    .is-active {
        border:3px #23d160 solid;
    }
</style>
