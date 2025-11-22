<script>
	import { createEventDispatcher, onMount } from "svelte";
    import enterSoundBase from "$lib/assets/enter.mp3";
    import typeSoundBase from "$lib/assets/typing.mp3";

    const dispatch = createEventDispatcher();
    export let command = "pip install dhananjayan";
    let typedEl;
    let cursorEl;
    let enterPressed = false;
    let enterSound;
    let canPlay = true; // avoid double presses
    let isWiping = false;

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
        isWiping = true;

		cursorEl.style.animation = "none";
	}

    function notifyDone() {
        dispatch("complete");
    }

    function handleEnter(){
        if (!canPlay) return;
        canPlay = false;

        enterSound.currentTime = 0;
        enterSound.play().catch(() => {});

        // after ~150ms reset
        setTimeout(() => canPlay = true, 150);
        triggerComplete();
    }

	function handleKeydown(e) {
		if (e.key === "Enter") handleEnter();
	}

    function loadAudio(){
        enterSound = new Audio(enterSoundBase); // put file in static/sounds
        enterSound.preload = "auto";
    }

    onMount(() => {
        typeText();
        loadAudio();
        window.addEventListener("keydown", handleKeydown);
    });
</script>

<div class="terminal" class:wipe-out={isWiping} onanimationend={notifyDone}>
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
		<div class="enter-key" onclick={handleEnter} onkeydown={handleKeydown} role="button" tabindex="0">ENTER</div>
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
    .wipe-out {
        animation: wipeDiagonal 0.75s cubic-bezier(0.22, 1, 0.36, 1) forwards;
        transform-origin: top right;
    }

    @keyframes wipeDiagonal {
        0% {
            opacity: 1;
            transform: translate(0, 0) skew(0deg, 0deg) scale(1);
            filter: blur(0px);
        }

        100% {
            opacity: 0;
            transform:
                translate(50px, 60px)   /* diagonal up-right */
                skew(-6deg, -2deg)       /* slight macOS space-switch skew */
                scale(0.97);             /* subtle shrink */
            filter: blur(10px);
        }
    }


</style>