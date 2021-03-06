<script context="module">
	/**
	 * @type {import('@sveltejs/kit').Load}
	 */
	export async function load({ page }) {
		return handleLoadQuery(
			browser && page.query,
			{
				filterValues: {
					actionTerm: '',
					category: [],
					country: [],
					department: [],
					level: [],
					region: [],
					subcategory: [],
					sourceTerm: ''
				}
			},
			(query) => ({
				filterValues: {
					actionTerm: query.get(InputNames.action),
					category: query.getAll(InputNames.category),
					country: query.getAll(InputNames.country),
					department: query.getAll(InputNames.department),
					level: query.getAll(InputNames.level).map((level) => parseInt(level, 10)),
					region: query.getAll(InputNames.region),
					subcategory: query.getAll(InputNames.subcategory),
					sourceTerm: query.get(InputNames.source)
				},
				query
			})
		);
	}
</script>

<script>
	import Filters from '$lib/components/Filters.svelte';
	import FiltersResults from '$lib/components/FiltersResults.svelte';
	import SectionIntroduction from '$lib/components/SectionIntroduction.svelte';
	import PageMeta from '$lib/components/PageMeta.svelte';
	import SectionBackground from '$lib/components/SectionBackground.svelte';
	import Backgrounds from '$lib/constants/backgrounds';
	import { aboutPage, actionsPage, actionsTreePage } from '$lib/constants/pages';
	import Section from '$lib/components/Section.svelte';
	import SectionContentList from '$lib/components/SectionContentList.svelte';
	import SectionContentListItem from '$lib/components/SectionContentListItem.svelte';
	import currentPage from '$lib/stores/currentPage';
	import FiltersDatabaseResults from '$lib/components/FiltersDatabaseResults.svelte';
	import { browser } from '$app/env';
	import getActionResults from '$lib/utils/getActionResults';
	import InputNames from '$lib/constants/input-names';
	import handleLoadQuery from '$lib/utils/handleLoadQuery';
	import Categories from '$lib/data/categories';
	import subcategories from '$lib/data/subcategories';

	export let filterValues;
	export let query;

	$: treeResults = getActionResults(filterValues);

	currentPage.set(actionsPage);
</script>

<PageMeta page={actionsPage} />

<SectionBackground background={Backgrounds.Lines}>
	<SectionIntroduction>
		S'inspirer de l'écosystème des possibles<br /> pour passer à l'action, ensemble
	</SectionIntroduction>
</SectionBackground>

<Section>
	<svelte:fragment slot="content">
		<SectionContentList>
			<li>
				<img
					alt=""
					class="floating-image"
					src="/illustrations/arbre.png"
					srcset="/illustrations/arbre.png 1x, /illustrations/arbre-2x.png 2x"
				/>
			</li>

			<SectionContentListItem link={actionsTreePage.path} linkStyle="primary">
				<svelte:fragment slot="title">L'arbre interactif</svelte:fragment>
				<svelte:fragment slot="text">
					Retrouve ou découvre la <strong>version interactive</strong> de l'affiche du
					<a href="{aboutPage.path}#{aboutPage.anchors.explanation}">guide papier</a>. Plus de
					<strong>150 actions concrètes</strong>
					sur les thèmes de l'énergie, du climat, de l'effondrement et de la résilience, organisées par
					thématique, comme les racines d'un arbre des possibles.<br /> Chaque action est associée à
					au moins une structure de référence (organisation, asso, collectif...) pour
					<strong>nous aider à nous mettre en mouvement</strong>, collectivement.
				</svelte:fragment>
				<svelte:fragment slot="link">Explorer l'arbre</svelte:fragment>
			</SectionContentListItem>

			<SectionContentListItem
				isExternalLink
				link="https://airtable.com/shrlqNJvuiem0iFkA"
				linkStyle="primary"
			>
				<svelte:fragment slot="title">Un site collaboratif</svelte:fragment>
				<svelte:fragment slot="text">
					La sélection des actions de l'arbre n'est <strong>pas exhaustive</strong> ! C'est une
					porte d'entrée à explorer ensemble.<br /> Ce site est collaboratif. Vos suggestions sont
					essentielles pour <strong>compléter cette liste</strong> avec des actions et structures
					toujours plus nombreuses, diverses et ancrées localement.<br /> C'est important de rendre
					visible le magnifique
					<strong>écosystème déjà bien vivant, évolutif</strong> au gré de nos élans d'actions.
				</svelte:fragment>
				<svelte:fragment slot="link">Faire grandir l'arbre</svelte:fragment>
			</SectionContentListItem>
		</SectionContentList>
	</svelte:fragment>
</Section>

<SectionBackground background={Backgrounds.Strokes}>
	<Section isCentered>
		<svelte:fragment slot="title"
			>Révéler les initiatives existantes, tisser une toile plus dense</svelte:fragment
		>
	</Section>
</SectionBackground>

<section class="container" id={actionsPage.anchors.filters}>
	<aside>
		<Filters values={filterValues} />
	</aside>

	<div>
		<FiltersResults results={treeResults} title="{treeResults.length} résultat(s) dans l'arbre" />

		{#if browser}
			<hr />
			<FiltersDatabaseResults {query} />
		{/if}
	</div>
</section>

<SectionBackground background={Backgrounds.Marks}>
	<Section>
		<svelte:fragment slot="title">Trouver du soutien quand on se sent seul·e</svelte:fragment>
		<svelte:fragment slot="content">
			<p>
				Votre entourage n'est pas sensible à ces questions ? Vous vous sentez en décalage ? Où que
				l'on soit sur son chemin de transition, il arrive de faire face à des questionnements, du
				désarroi, ou un sentiment d'impuissance qui freinent la mise en action. C'est le cas pour
				nombre d'entre nous ! Alors voici quelques pistes pour commencer :
			</p>
			<SectionContentList nbItemsByLine={2}>
				<SectionContentListItem
					isExternalLink
					link="http://www.ateliersdetravailquirelie.sitew.fr/"
					linkStyle="primary"
				>
					<svelte:fragment slot="text">
						L'accompagnement du film <i>Une fois que tu sais</i> s’inspire du
						<i>Travail Qui Relie</i>
						(TQR), un processus développé par l'écopsychologue Joanna Macy, qui
						<strong>soutient la traversée des émotions difficiles</strong> liées à l'état du monde, pour
						les transformer en élan d'action. Des ateliers accessibles à tou·tes sont proposés toute
						l’année, un peu partout.
					</svelte:fragment>
					<svelte:fragment slot="link">Découvrir les Ateliers TQR</svelte:fragment>
				</SectionContentListItem>

				<SectionContentListItem
					link="{actionsPage.path}?{InputNames.subcategory}={subcategories.imagination
						.slug}&{InputNames.subcategory}={subcategories.uncertainty
						.slug}&{InputNames.subcategory}={subcategories.living
						.slug}&{InputNames.subcategory}={subcategories.conflicts.slug}#{actionsPage.anchors
						.filters}"
					linkStyle="primary"
				>
					<svelte:fragment slot="text">
						De nombreuses actions, structures et initiatives de la catégorie <i>Régénérer</i> sur
						notre page
						<a
							href="{actionsPage.path}?{InputNames.category}={Categories.Regenerate}#{actionsPage
								.anchors.filters}">Actions</a
						> proposent des ressources, des groupes, et des espaces pour trouver du soutien.
					</svelte:fragment>
					<svelte:fragment slot="link">Découvrir les actions</svelte:fragment>
				</SectionContentListItem>
			</SectionContentList>
		</svelte:fragment>
	</Section>
</SectionBackground>

<style lang="scss">
	li {
		@include utilities.media-sm-md {
			text-align: center;
		}
		@include utilities.media-md {
			grid-column: 1 / 3;
		}
		img {
			height: 100%;
			object-fit: cover;

			@include utilities.media-sm {
				width: 50%;
			}
			@include utilities.media-md {
				width: 40%;
			}
			@include utilities.media-lg {
				bottom: -8%;
				left: -8%;
				position: relative;
				width: 100%;
			}
		}
	}
	section {
		@include utilities.responsive-grid(1fr, 1fr, 1fr 2fr);

		align-items: start;
		padding-bottom: 8rem;
		padding-top: utilities.$header-height;

		@include utilities.media-sm {
			padding-top: utilities.$header-height-sm;
		}

		aside {
			background: var(--bg-secondary-color);
			padding: 1rem 2rem;
		}

		div hr {
			margin: 2rem 0;
		}
	}
</style>
