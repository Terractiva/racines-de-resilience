<script>
	import handleDraggedLink from '$lib/actions/handleDraggedLink';
	import InputNames from '$lib/constants/input-names';
	import { actionsPage } from '$lib/constants/pages';
	import { treeSubcategoryFontSize } from '$lib/constants/settings';

	export let backgroundHeight;
	export let isDisabled;
	export let subcategory;
</script>

<li
	style="font-size: {treeSubcategoryFontSize * backgroundHeight}px; left: {subcategory
		.position[0]}%; top: {subcategory
		.position[1]}%; width: {subcategory.width}%; height: {subcategory.height}%;"
>
	<a
		use:handleDraggedLink={isDisabled}
		href="{actionsPage.path}?{encodeURIComponent(
			InputNames.subcategory
		)}={subcategory.slug}#{actionsPage.anchors.filters}"
		target="_blank"
	>
		<b>
			{subcategory.name}
		</b>
	</a>
</li>

<style lang="scss">
	li {
		align-items: center;
		display: flex;
		justify-content: center;
		position: absolute;
		text-align: center;

		a {
			color: var(--font-color);
			line-height: 1.1;
		}
	}
</style>
