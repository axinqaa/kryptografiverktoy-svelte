<script>
	import { page } from '$app/state';

	let { children } = $props();
</script>

<nav>
	<a href="/" class:aktiv={page.url.pathname === '/'}>Hjem</a>
	<a href="/caesar" class:aktiv={page.url.pathname === '/caesar'}>Caesar</a>
	<a href="/vigenere" class:aktiv={page.url.pathname === '/vigenere'}>Vigenère</a>
	<a href="/otp" class:aktiv={page.url.pathname === '/otp'}>One-Time Pad</a>
</nav>

<main>
	{@render children()}
</main>

<style>
	:global(body) {
		margin: 0;
		font-family: Arial, sans-serif;
		background-color: #f4f4f4;
	}
	nav {
		background-color: #333;
		padding: 12px 20px;
	}
	nav a {
		color: #ccc;
		margin-right: 20px;
		text-decoration: none;
	}
	nav a.aktiv {
		color: white;
		font-weight: bold;
		border-bottom: 2px solid white;
	}
	main {
		max-width: 600px;
		margin: 20px auto;
		padding: 0 15px;
	}

	/* felles stil som brukes av alle metode-sidene */
	main :global(textarea),
	main :global(input[type='text']) {
		width: 100%;
		box-sizing: border-box;
		padding: 6px;
		font-size: 16px;
	}
	main :global(button) {
		margin: 12px 8px 0 0;
		padding: 8px 16px;
		font-size: 16px;
		cursor: pointer;
	}
	main :global(.feil) {
		color: #b00020;
		background-color: #fde8e8;
		padding: 8px;
	}
	main :global(.resultat) {
		font-family: monospace;
		font-size: 18px;
		background-color: white;
		border: 1px solid #ccc;
		padding: 8px;
		word-break: break-all;
	}
	main :global(.nokkel) {
		background-color: #fff8c5;
	}
</style>
