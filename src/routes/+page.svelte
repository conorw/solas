<script>
	import { page } from '$app/stores'
	import Account from './auth/profile/+page.svelte'
	import Auth from './auth/login/Auth.svelte'

    console.log('PARAMS', {params: $page.url})
</script>

<svelte:head>
	<title>Solas Attendance Tracker</title>
	<meta name="description" content="Solas Tracker" />
</svelte:head>

{#if !$page.data.session}
	<Auth />
{:else}
	Welcome
{/if}