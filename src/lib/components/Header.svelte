<script>
	import {
		aboutPage,
		actionsPage,
		onceYouKnowPage,
		homePage,
		supportUsPage
	} from '$lib/constants/pages';
	import Themes from '$lib/constants/themes';
	import theme from '$lib/stores/theme';
	import ThemeSwitcher from './ThemeSwitcher.svelte';

	export let currentPath;
	export let hideMenu;
	export let isFullScreen;

	let isMenuOpen = false;
	const pages = [actionsPage, supportUsPage, onceYouKnowPage, aboutPage];
</script>

<header class:has-background={!isFullScreen}>
	<a href={homePage.path}>
		<img
			alt="Racines de résilience"
			class:is-dark={$theme.value === Themes.Dark}
			src="/logo.png"
			srcset="/logo.png 1x, /logo-2x.png 2x"
		/>
	</a>
</header>

<nav class="nav">
	<button class="button icon-only" type="button" on:click={() => (isMenuOpen = !isMenuOpen)}>
		<svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"
			><path
				d="M2 6a1 1 0 0 1 1-1h18a1 1 0 1 1 0 2H3a1 1 0 0 1-1-1zM2 12.032a1 1 0 0 1 1-1h18a1 1 0 1 1 0 2H3a1 1 0 0 1-1-1zM3 17.064a1 1 0 1 0 0 2h18a1 1 0 0 0 0-2H3z"
				fill="currentColor"
			/></svg
		>
	</button>

	<div class="tabs" class:is-open={isMenuOpen}>
		{#if hideMenu !== true}
			<a class:active={currentPath === '/'} href="/" on:click={() => (isMenuOpen = false)}>
				<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
					<path
						d="M10.707 2.293a1 1 0 00-1.414 0l-7 7a1 1 0 001.414 1.414L4 10.414V17a1 1 0 001 1h2a1 1 0 001-1v-2a1 1 0 011-1h2a1 1 0 011 1v2a1 1 0 001 1h2a1 1 0 001-1v-6.586l.293.293a1 1 0 001.414-1.414l-7-7z"
					/>
				</svg>
			</a>
			{#each pages as page}
				<a
					class:active={page.path === currentPath}
					href={page.path}
					on:click={() => (isMenuOpen = false)}>{page.label}</a
				>
			{/each}
		{/if}
		<ThemeSwitcher />
	</div>
</nav>

<style lang="scss">
	header {
		&.has-background {
			background: var(--bg-color);
		}

		a {
			display: block;
			height: 100%;
			padding: 0.5rem 1rem;

			img {
				height: 100%;
				width: auto;

				&.is-dark {
					filter: brightness(100);
				}
			}
		}
	}

	nav {
		button {
			@include utilities.media-sm {
				margin: auto 0.2rem;
				padding: 0.5rem;
			}
			@include utilities.media-md {
				margin: auto 0.4rem;
			}
			@include utilities.media-lg {
				display: none;
			}

			svg {
				display: block;
				height: 2rem;
				width: 2rem;
			}
		}

		.tabs {
			background: var(--bg-color);
			box-shadow: utilities.$shadow;
			display: flex;

			a {
				padding: 1rem 1.5rem;

				svg {
					display: block;
					height: 2rem;
					width: 2rem;
				}
			}

			@include utilities.media-sm-md {
				flex-direction: column;
				align-items: center;
				position: absolute;
				right: 0;
				top: 100%;

				&:not(.is-open) {
					display: none;
				}

				a {
					text-align: initial;
					width: 100%;

					svg {
						margin: 0 auto;
					}
				}
			}
		}
	}
</style>
