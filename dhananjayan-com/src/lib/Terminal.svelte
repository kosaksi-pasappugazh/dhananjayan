<script>
	import { createEventDispatcher, onMount } from "svelte";

    const dispatch = createEventDispatcher();
    export let command = "pip install myname";
    let typedEl;
    let cursorEl;
    let enterPressed = false;

    async function typeText() {
        for (let char of command){
            const delay = 40 + Math.random() * 100;
            await new Promise(r => setTimeout(r, delay));
            typedEl.innerHTML += `<span class="fade-char">${char}</span>`;
        } 
    }

	function triggerComplete() {
		if (enterPressed) return;
		enterPressed = true;

		cursorEl.style.animation = "none";

		dispatch("complete");
	}

	function handleKeydown(e) {
		if (e.key === "Enter") triggerComplete();
	}

    onMount(() => {
        window.addEventListener("keydown", handleKeydown);
        typeText();
    });
</script>

<div class="terminal">
    <div class="header">
        <span class="dot red"></span>
        <span class="dot yellow"></span>
        <span class="dot green"></span>
    </div>
    <div class="body">
        <div class="line">
            <span class="prompt">$</span>
            <span class="typed" bind:this={typedEl}></span>
            <span class="cursor" bind:this={cursorEl}>|</span>
        </div>
    </div>
    <div class="keyboard">
		<div class="enter-key" on:click={triggerComplete}>ENTER</div>
	</div>
</div>

<style>
    .terminal {
        width: 100%;
        height: 100%;
        inset: 0;
        position: fixed;
        background-color: #111;
        color: #eee;
        overflow: hidden;
        font-family: "JetBrains Mono",monospace;
        border-radius: 8px;
        /* padding: 1rem; */
        box-shadow: 0 10px 30px rgba(0,0,0,0.35);
        display: flex;
        flex-direction: column;
    }
    .header {
        background: #222;
        padding: 8px 12px;
        display: flex;
        gap: 8px;
        align-items: center;
    }
    .dot {
        width: 12px;
        height: 12px;
        border-radius: 50%;
        display: inline-block;
    }
    .red { background: #ff5f56; }
    .yellow { background: #ffbd2e; }
    .green { background: #27c93f; }
    .body {
        padding: 1rem;
        flex: 1;
        display: flex;
        align-items: flex-start;
    }
    .line {
        display: flex;
        align-items: center;
    }
    pre {
        margin: 0;
        line-height: 1.5;
        font-size: 15px;
        white-space: pre-wrap;
        position: relative;
        padding-left: 24px;
        /* color: #d4d4d4; */
    }
    .prompt{
        content: "$";
        color: #27c93f;
        font-weight: bold;
        position: absolute;
        margin-right: 6px;
        user-select: none;
        left: 0;
        opacity: 0.7;
    }
    .cursor {
        margin-left: 2px;
        animation: blink 1s step-end infinite;
    }
    @keyframes blink {
        50% { opacity: 0; }
    }
    .fade-char {
        opacity: 0;
        animation: fadein 0.15s forwards;
    }

    @keyframes fadein {
        to { opacity: 1; }
    }
	.keyboard {
		padding: 1rem;
		display: flex;
		justify-content: center;
	}

	.enter-key {
		background: #1b1b1b;
		border: 2px solid #333;
		padding: 1rem 2.5rem;
		border-radius: 8px;
		cursor: pointer;
		box-shadow: 0 6px 0 #000;
		transform: perspective(200px) translateZ(20px);
		transition: transform 0.1s ease, box-shadow 0.1s ease;
	}

	.enter-key:active {
		transform: perspective(200px) translateZ(5px);
		box-shadow: 0 2px 0 #000;
	}
</style>