<script>
	import Terminal from "$lib/Terminal.svelte";

	let introDone = false;
	let hideTerminal = false;

	function handleTerminalFinish() {
		hideTerminal = true;

		setTimeout(() => {
			introDone = true;
		}, 600); // match fade-out duration
	}
</script>

<div class="container">
	{#if !introDone}
		<div class:fadeOut={hideTerminal}>
			<Terminal on:complete={handleTerminalFinish}/>
		</div>
	{/if}

	{#if introDone}
		<div class="landing fadeIn">
			<h1>Welcome to my portfolio ✨</h1>
			<p>This page lives entirely in +page.svelte now.</p>
		</div>
	{/if}
</div>

<style>
	.container {
		position: relative;
		width: 100%;
		height: 100vh;
		overflow: hidden;
	}

	.fadeOut {
		opacity: 0;
		transform: translateY(-30px);
		transition: opacity 0.6s ease, transform 0.6s ease;
	}

	.fadeIn {
		opacity: 0;
		animation: fadeSlideIn 0.8s forwards;
	}

	@keyframes fadeSlideIn {
		from { opacity: 0; transform: translateY(20px); }
		to { opacity: 1; transform: none; }
	}
</style>
