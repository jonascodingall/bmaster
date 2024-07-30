<script lang="ts">
	import Login from '$lib/components/login.svelte';
	import Register from '$lib/components/register.svelte';

	function loginHandler(e: CustomEvent<{ email: string; password: string }>): void {
		console.log(e.detail);
	}

	function registerHandler(
		e: CustomEvent<{ username: string; email: string; password: string; confirmPassword: string }>
	): void {
		console.log(e.detail);
	}
</script>

<div class="wrapper">
	<Register on:register={registerHandler} />
	<Login on:login={loginHandler} />
</div>

<style lang="postcss">
	.wrapper {
		@apply flex h-screen justify-center;
	}
</style>
