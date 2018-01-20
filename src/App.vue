<template lang="pug">
    #app
        section.section
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
            .container
                .columns
                    .column(v-for="tr in tracks") {{tr.name}} - {{tr.artists[0].name}}

</template>

<script>
// const tracks = [
//     {name: 'Caraluna', artist:'Bacano'},
//     {name: 'Dusk till dawn', artist:'Sia ft Zyan'},
//     {name: 'i was made for loving you', artist: 'The kiss'},
// ]

import trackService from './services/track'
export default {
    name: 'app',
    data () {
        return {
            searchQuery: '',
            tracks: []

        }
    },
    methods: {
        search() {
            if (!this.searchQuery) {return}

            trackService.search(this.searchQuery)
            .then((res)=> {
                this.tracks = res.tracks.items;
            })
        }
    },
    computed: {
        searchMessage() {
            return `Se encontraron ${this.tracks.length} resultados.`
        }
    }
}
</script>

<style lang="scss">
@import './scss/main.scss';

</style>
