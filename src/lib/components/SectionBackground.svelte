<script>
	import Themes from '$lib/constants/themes';
	import theme from '$lib/stores/theme';

	export let background;
</script>

<div class={`bg-${background}`} class:is-dark={$theme.value === Themes.Dark}>
	<slot />
</div>

<style lang="scss">
	@mixin background-variant($bg-name, $filename, $filename-2x, $opacity-light, $opacity-dark) {
		&.bg-#{$bg-name} {
			&.is-dark::before {
				opacity: $opacity-dark;
			}
			&:not(.is-dark)::before {
				opacity: $opacity-light;
			}

			&::before {
				background-image: url('/fonds/#{$filename}');

				@include utilities.media-2x {
					background-image: url('/fonds/#{$filename-2x}');
				}
			}
		}
	}

	div {
		position: relative;

		/* Ne pas oublier de mettre à jour l'option `safelist` de `purgecss` dans `svelte.config.js` */
		@include background-variant('lines', 'lignes.png', 'lignes-2x.png', 0.3, 0.6);
		@include background-variant('marks', 'marques.png', 'marques-2x.png', 0.2, 0.9);
		@include background-variant('strokes', 'traits.png', 'traits-2x.png', 0.15, 0.7);

		&::before {
			background-repeat: repeat;
			background-size: 250px auto;
			content: '';
			height: 100%;
			position: absolute;
			width: 100%;
			z-index: -1;
		}
	}
</style>
