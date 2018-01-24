<template lang="pug">
	section.hero.is-success
		.hero-head
			header.nav
				.container
					.nav-left
						-nav-item
							strong Music - Platzi Music
					.nav-right.nav-menu
						router-link.nav-item(to="/") Buscar
						router-link.nav-item(to="about") Nosotros

		.hero-body
			.container.has-text-centered
				h1.title PlatziMusic
				h2.subtitle Escucha lo que te husta
				pm-player
</template>

<script>

import PmPlayer from '@/component/Player.vue'

export default {
	components: {PmPlayer}
}
</script>