<script lang="ts">
	import { supabaseClient } from '$lib/supabaseClient';
	import { page } from '$app/stores'
	import { goto, invalidate } from '$app/navigation';
	import { onMount } from 'svelte';
	import './styles.css';
	import Auth from './auth/login/Auth.svelte';

	onMount(() => {
		const {
			data: { subscription }
		} = supabaseClient.auth.onAuthStateChange((event) => {
			console.log('auth state changed', event);
			invalidate('supabase:auth');
			if (event === 'PASSWORD_RECOVERY') {
				console.log('password recovery');
				goto('/auth/reset');
			}
		});

		return () => {
			subscription.unsubscribe();
		};
	});
</script>

<nav>
	<a href="/">Home</a>
	<a href="/auth/profile">Profile</a>
</nav>
{#if !$page.data.session}
	<Auth />
{:else}
	<slot />
{/if}

