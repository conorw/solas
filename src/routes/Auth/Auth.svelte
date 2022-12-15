<script lang="ts">
	import { supabaseClient } from '$lib/supabaseClient';
	import { page } from '$app/stores';
	import { goto } from '$app/navigation';
	// import Auth from 'supabase-ui-svelte'
	let loading = false;
	let email: string = $page.url.searchParams.get('email') || '';
	let password: string;
    console.log('PARAMS', {params: $page.url})
	let view: any = $page.url.searchParams.get('type') || 'sign_in';
	let accessToken: any = $page.url.searchParams.get('access_token') || '';

	const handleLogin = async () => {
		try {
			loading = true;

			if (view === 'sign_in') {
				const { error } = await supabaseClient.auth.signInWithPassword({ email, password });
				if (error) throw error;
				alert('Check your email for the login link!');
			} else {
				const { error } = await supabaseClient.auth.updateUser({ password });
                goto('/')
				if (error) throw error;
				alert('Check your email for the login link!');
			}
		} catch (error) {
			if (error instanceof Error) {
				alert(error.message);
			}
		} finally {
			loading = false;
		}
	};
</script>

<form class="row flex-center flex" on:submit|preventDefault={handleLogin}>
	<div class="col-6 form-widget">
		<h1 class="header">Solas Attendance Tracker</h1>
		{#if view === 'sign_in'}
			<p class="description">Sign in with email and password below</p>
			<div>
				<input class="inputField" type="email" placeholder="Your email" bind:value={email} />
			</div>
		{/if}
		<div>
			<input class="inputField" type="password" placeholder="Your password" bind:value={password} />
		</div>
		<div>
			<input
				type="submit"
				class="button block"
				value={loading ? 'Loading' : 'Login'}
				disabled={loading}
			/>
		</div>
	</div>
</form>
