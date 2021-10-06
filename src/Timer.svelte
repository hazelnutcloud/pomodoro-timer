<script>
    export let inSession;
    export let timeLeft;
    export let handleTimer;

    $: minutes = Math.floor(timeLeft / 60);
	$: seconds = (timeLeft % 60).toString().padStart(2, '0');
</script>

<div class="container">
    <div class="label-container">
        <div id="timer-label">
            <h1>
                {#if inSession}
                    Session
                {:else}
                    Break
                {/if}
            </h1>
        </div>
        <div id="time-left">
            {minutes}:{seconds}
        </div>
    </div>
    <div class="button-container">
        <button id="reset" on:click={handleTimer.reset}>
            Reset Timer
        </button>
        <button id="start_stop" on:click={handleTimer.startStop}>
            Start / Stop
        </button>
    </div>
</div>

<style>
    h1 {
        margin: 0;
    }
    .container {
        width: 100%;
        text-align: center;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        align-items: center;
        height: 300px;
    }
    .label-container {
        width: 100%;
        max-width: 230px;
        background: rgba(255, 255, 255, 0.2);
        border-radius: 16px;
        box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
        backdrop-filter: blur(5px);
        -webkit-backdrop-filter: blur(5px);
        border: 1px solid rgba(255, 255, 255, 0.3);
        padding: 2em;
        box-sizing: border-box;
        margin: 0.5em;
    }
    .button-container {
        display: flex;
        gap: 1em;
        justify-content: center;
    }
    #time-left {
        font-size: xx-large;
        font-weight: bolder;
    }
</style>