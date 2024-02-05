<!-- <script>
	import {
		Card,
		Button,
		Label,
		Input,
		Checkbox,
		ButtonGroup,
		InputAddon,
		ToolbarButton,
		Modal
	} from 'flowbite-svelte';
	import { EyeOutline, EyeSlashOutline } from 'flowbite-svelte-icons';
	let show = false;
	let signed = false;
	let username = '';
	let password = '';

	async function signin() {
		try {
			const formData = new FormData();
			formData.append('username', username);
			formData.append('password', password);

			const response = await fetch('http://localhost:8000/api/signin', {
				method: 'POST',
				body: formData
			});

			if (response.ok) {
				const responseData = await response.json();
				const token = responseData.jwt;
				localStorage.setItem('jwt', token);
			}
		} catch (error) {
			console.log(error);
		}
	}
</script>

<div class="flex min-h-screen items-center justify-center bg-white">
	<Card class="w-full max-w-md">
		<form class="flex flex-col space-y-6">
			<h3 class="text-xl font-medium text-gray-900 dark:text-white">Admin Signin Only</h3>
			<Label class="space-y-2">
				<span>Username</span>
				<Input type="text" name="username" placeholder="Username" bind:value={username} required />
			</Label>
			<div>
				<Label for="show-password" class="mb-2">Your password</Label>
				<Input
					id="show-password"
					type={show ? 'text' : 'password'}
					placeholder="Your password here"
					size="lg"
					bind:value={password}
				>
					<button slot="left" on:click={() => (show = !show)} class="pointer-events-auto">
						{#if show}
							<EyeOutline class="h-6 w-6" />
						{:else}
							<EyeSlashOutline class="h-6 w-6" />
						{/if}
					</button>
				</Input>
			</div>
			<Button type="submit" class="w-full" on:click={() => signin()}>Login to your account</Button>

			{#if signed}
				<Modal title="you are successfully registered !" bind:open={signed} autoclose>
					<div>Signed in Successfully</div>
				</Modal>
			{/if}

			<div class="text-sm font-medium text-gray-500 dark:text-gray-300">
				Not registered? <a
					href="/signup"
					class="text-primary-700 hover:underline dark:text-primary-500"
				>
					Create account
				</a>
			</div>
		</form>
	</Card>
</div> -->
