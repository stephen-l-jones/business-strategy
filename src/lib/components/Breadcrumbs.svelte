<script lang="ts">
	import { base } from '$app/paths';

	interface Breadcrumb {
		label: string;
		href: string;
	}

	export let path: string;
	export let navPages: NavPage[];
	let breadcrumbs: Breadcrumb[] = [];

	$: {
		// Remove zero-length tokens.
		let tokens = path.split('/').filter((x) => !!x);
		if (base !== '' || path === '/') tokens.shift();

		// Create { label, href } pairs for each token.
		let tokenPath: string = '';
		breadcrumbs = tokens.map((token) => {
			let label: string;

			tokenPath += '/' + token;
			if (token === 'cases') {
				label = 'Cases';
			} else if (token === 'articles') {
				label = 'Articles';
			} else if (token === 'scenario') {
				label = 'Scenario';
			} else {
				label = navPages.find((navPage) => navPage.slug === token)?.name || 'X';
			}
			return {
				label: label,
				href: tokenPath
			};
		});
	}
</script>

<div class="breadcrumbs">
	<a href="/">◯</a>
	{#each breadcrumbs as breadcrumb, i}
		&nbsp;&gt;&nbsp;
		{#if i == breadcrumbs.length - 1}
			{breadcrumb.label}
		{:else}
			<a href={breadcrumb.href}>{breadcrumb.label}</a>
		{/if}
	{/each}
</div>

<style>
	:global(.breadcrumbs) {
		font-size: 1.1rem;
	}
</style>
