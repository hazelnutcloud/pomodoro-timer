<script>
	import { writable } from "svelte/store";
	import TimeControl from "./TimeControl.svelte";
	import Timer from "./Timer.svelte";

	const sessionControl = {
		name: "session",
		label: "Session Length",
		defaultValue: 25,
		userValue: 25,
		increment: () => {
			sessionControl.userValue =
				sessionControl.userValue === 60
					? 60
					: sessionControl.userValue + 1;
			if (inSession && !timerRan)
				timeLeft.update((t) => (t === 60 * 60 ? 60 * 60 : t + 1 * 60));
		},
		decrement: () => {
			sessionControl.userValue =
				sessionControl.userValue === 1
					? 1
					: sessionControl.userValue - 1;
			if (inSession && !timerRan)
				timeLeft.update((t) => (t === 1 * 60 ? 1 * 60 : t - 1 * 60));
		},
	};
	const breakControl = {
		name: "break",
		label: "Break Length",
		defaultValue: 5,
		userValue: 5,
		increment: () => {
			breakControl.userValue =
				breakControl.userValue === 60 ? 60 : breakControl.userValue + 1;
			if (!inSession && !timerRan)
				timeLeft.update((t) => (t === 60 * 60 ? 60 * 60 : t + 1 * 60));
		},
		decrement: () => {
			breakControl.userValue =
				breakControl.userValue === 1 ? 1 : breakControl.userValue - 1;
			if (!inSession && !timerRan)
				timeLeft.update((t) => (t === 1 * 60 ? 1 * 60 : t - 1 * 60));
		},
	};

	const timeLeft = writable(1500);
	let inSession = true;
	let timer;
	let timerRunning = false;
	let timerRan = false;
	let audio;

	const handleTimer = {
		startStop: () => {
			timerRan = true;
			if (timerRunning) {
				clearInterval(timer);
				console.log("timer paused");
				timerRunning = false;
			} else {
				timer = setInterval(() => timeLeft.update((t) => t - 1), 1000);
				console.log("timer started");
				timerRunning = true;
			}
		},
		reset: () => {
			timerRan = false;
			clearInterval(timer);
			inSession = true;
			sessionControl.userValue = sessionControl.defaultValue;
			breakControl.userValue = breakControl.defaultValue;
			$timeLeft = sessionControl.defaultValue * 60;
			timerRunning = false;
			audio.pause();
			audio.load();
		},
	};

	$: if ($timeLeft === 0) {
		inSession = !inSession;
		if (inSession) timeLeft.set(sessionControl.userValue * 60);
		else timeLeft.set(breakControl.userValue * 60);
		audio.play();
	}
</script>

<div class="container">
	<Timer timeLeft={$timeLeft} {handleTimer} {inSession} />
	<div class="time-controls-container">
		<TimeControl timeControl={sessionControl} />
		<TimeControl timeControl={breakControl} />
	</div>
</div>
<p class="credits">Made with 🧡 by <a href="https://twitter.com/hsnmkls">Hasan Mukhlis</a></p>
<audio
	id="beep"
	preload="auto"
	src="https://raw.githubusercontent.com/freeCodeCamp/cdn/master/build/testable-projects-fcc/audio/BeepSound.wav" 
	bind:this={audio}
/>

<style>
	:global(button) {
		background: rgba(255, 255, 255, 0.2);
		border-radius: 16px;
		box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
		backdrop-filter: blur(5px);
		-webkit-backdrop-filter: blur(5px);
		border: 1px solid rgba(255, 255, 255, 0.3);
		text-align: center;
		padding: 1em;
	}

	.container {
		width: 100%;
		max-width: 800px;
		display: flex;
		gap: 1em;
		align-items: center;
		background: rgba(255, 255, 255, 0.2);
		border-radius: 16px;
		box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
		backdrop-filter: blur(5px);
		-webkit-backdrop-filter: blur(5px);
		border: 1px solid rgba(255, 255, 255, 0.3);
		padding: 2em;
		box-sizing: border-box;
		margin: 1em;
	}
	@media screen and (max-width: 600px) {
		.container {
			flex-direction: column;
			align-items: center;
		}
	}
	.time-controls-container {
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		gap: 2em;
	}
	.credits {
		text-align: center;
		margin-top: 1em;
		font-size: 0.8em;
	}
</style>
