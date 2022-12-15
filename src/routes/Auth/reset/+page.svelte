<script lang="ts">
	import { supabaseClient } from '$lib/supabaseClient';
	import { page } from '$app/stores';
	import { goto } from '$app/navigation';
	// import Auth from 'supabase-ui-svelte'
	let loading = false;
	let password: string;

	const handleLogin = async () => {
		try {
			loading = true;
			const { error } = await supabaseClient.auth.updateUser({ password });
			if (error) throw error;
			await supabaseClient.auth.signOut();
			goto('/');
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
		<p class="description">Update password</p>
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
