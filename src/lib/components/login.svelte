<script lang="ts">
	import { lock_closed_outline, mail_outline } from '$lib/svg/ionicons.js';
	import { Card, Input, Button } from 'stwui';
	import { createEventDispatcher } from 'svelte';

	// function props
	const dispatch = createEventDispatcher<{ login: { email: string; password: string } }>();

	// email
	let email = '';
	const emailPattern = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;
	let isEmailValid = '';

	function testEmail() {
		if (!emailPattern.test(email) && email.length > 0) {
			isEmailValid = 'Wrong Email format';
		} else {
			isEmailValid = '';
		}
	}

	function emailChangeHandler() {
		isEmailValid = '';
	}

	$: {
		email;
		emailChangeHandler();
	}

	// password
	let password = '';

	// form
	function submitHandler() {
		testEmail();
		if (isEmailValid == '') {
			console.log('hä');
			dispatch('login', {
				email: email,
				password: password
			});
		}
	}
</script>

<div class="cardWrapper">
	<form on:submit={submitHandler}>
		<Card>
			<Card.Header slot="header">Login</Card.Header>
			<Card.Content slot="content">
				<!--type="email" -> email autocomplete -->
				<Input
					error={isEmailValid}
					autocomplete="off"
					type="text"
					name="email"
					bind:value={email}
					on:change={emailChangeHandler}
				>
					<Input.Label slot="label">Email</Input.Label>
					<Input.Leading slot="leading" data={mail_outline} />
				</Input>
				<Input
					autocomplete="off"
					type="password"
					name="password"
					showPasswordToggle
					bind:value={password}
				>
					<Input.Label slot="label">Password</Input.Label>
					<Input.Leading slot="leading" data={lock_closed_outline} />
				</Input>
			</Card.Content>
			<Card.Actions slot="actions">
				<Button class="w-full" type="primary" htmlType="submit">Login</Button>
			</Card.Actions>
		</Card>
	</form>
</div>

<style lang="postcss">
	.cardWrapper {
		@apply m-auto flex;
	}
</style>
