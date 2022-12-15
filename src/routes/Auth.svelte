<script lang="ts">
	import { supabaseClient } from '$lib/supabaseClient'

	let loading = false
	let email: string
    let password: string

	const handleLogin = async () => {
		try {
			loading = true
			const { error } = await supabaseClient.auth.signInWithPassword({ email, password })
			if (error) throw error
			alert('Check your email for the login link!')
		} catch (error) {
			if (error instanceof Error) {
				alert(error.message)
			}
		} finally {
			loading = false
		}
	}
</script>

<form class="row flex-center flex" on:submit|preventDefault={handleLogin}>
	<div class="col-6 form-widget">
		<h1 class="header">Solas Attendance Tracker</h1>
		<p class="description">Sign in with email and password below</p>
		<div>
			<input class="inputField" type="email" placeholder="Your email" bind:value={email} />
		</div>
        <div>
			<input class="inputField" type="password" placeholder="Your password" bind:value={password} />
		</div>
		<div>
			<input
				type="submit"
				class="button block"
				value={loading ? 'Loading' : 'Send magic link'}
				disabled={loading}
			/>
		</div>
	</div>
</form>