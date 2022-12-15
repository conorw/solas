<script>
	import { page } from '$app/stores'
	import Account from './Auth/Account.svelte'
	import Auth from './Auth/Auth.svelte'
</script>

<svelte:head>
	<title>Solas Attendance Tracker</title>
	<meta name="description" content="Solas Tracker" />
</svelte:head>

{#if !$page.data.session || $page.url.searchParams.get('type') === 'recovery'}
	<Auth />
{:else}
	<Account session={$page.data.session} />
{/if}