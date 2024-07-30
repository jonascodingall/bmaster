<script lang="ts">
	import { lock_closed_outline, mail_outline, person_outline } from '$lib/svg/ionicons.js';
	import { Card, Input, Button } from 'stwui';
	import { createEventDispatcher } from 'svelte';

	// function props
	const dispatch = createEventDispatcher<{
		register: {
			username: string;
			email: string;
			password: string;
			confirmPassword: string;
		};
	}>();

	// username
	let username = '';

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
	let confirmPassword = '';
	let isPasswordValid = '';

	function testPassword() {
		if (password !== confirmPassword) {
			isPasswordValid = 'The Conformation does not match';
		} else {
			isPasswordValid = '';
		}
	}

	function passwordChangeHandler() {
		isPasswordValid = '';
	}

	$: {
		password;
		confirmPassword;
		passwordChangeHandler();
	}

	//
	function submitHandler() {
		testEmail();
		testPassword();
		if (isEmailValid == '' && isPasswordValid == '') {
			dispatch('register', {
				username: username,
				email: email,
				password: password,
				confirmPassword: confirmPassword
			});
		}
	}
</script>

<div class="cardWrapper">
	<form on:submit={submitHandler}>
		<Card>
			<Card.Header slot="header">Register</Card.Header>
			<Card.Content slot="content">
				<Input autocomplete="off" type="text" name="name" bind:value={username}>
					<Input.Label slot="label">Username</Input.Label>
					<Input.Leading slot="leading" data={person_outline} />
				</Input>
				<!--type="email" -> email autocomplete -->
				<Input autocomplete="off" type="text" name="email" bind:value={email} error={isEmailValid}>
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
				<Input
					autocomplete="off"
					type="password"
					name="password"
					showPasswordToggle
					bind:value={confirmPassword}
					error={isPasswordValid}
				>
					<Input.Label slot="label">Confirm Password</Input.Label>
					<Input.Leading slot="leading" data={lock_closed_outline} />
				</Input>
			</Card.Content>
			<Card.Actions slot="actions">
				<Button class="w-full" type="primary" htmlType="submit">Register</Button>
			</Card.Actions>
		</Card>
	</form>
</div>

<style lang="postcss">
	.cardWrapper {
		@apply m-auto flex;
	}
</style>
