<script>
	import { fetchData, pageQuery } from '$lib';
	import Post from '../../components/Post.svelte';
	import PostSkeleton from '../../components/PostSkeleton.svelte';

	export let data;
	let query;

	$: {
		query = fetchData(pageQuery([[data.props.slug.toUpperCase(), data.props.slug]], 40));
	}
</script>

<svelte:head>
	<title>hkr.news | {data.props.slug}</title>
</svelte:head>

{#await query}
	{#each Array(40) as _}
		<PostSkeleton />
	{/each}
{:then response}
	{#each response[data.props.slug].edges as { node }}
    {#if !node.dead && !node.deleted}
		  <Post {node} />
    {/if}
	{/each}
{/await}
