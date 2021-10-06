<script>
    export let timeControl;
</script>

<div class="container">
    <div id="{timeControl.name}-label" class="label">
        {timeControl.label}
    </div>
    <button id="{timeControl.name}-decrement" on:click={timeControl.decrement}
        >-</button
    >
    <div id="{timeControl.name}-length">{timeControl.userValue}</div>
    <button id="{timeControl.name}-increment" on:click={timeControl.increment}
        >+</button
    >
</div>

<style>
    button {
        padding: 0.2em;
    }
    .container {
        display: grid;
        max-width: 200px;
        grid-template-columns: repeat(3, minmax(20px, 80px));
        grid-template-rows: auto auto;
        padding: 2em;
/* From https://css.glass */
        background: rgba(255, 255, 255, 0.2);
        border-radius: 16px;
        box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
        backdrop-filter: blur(5px);
        -webkit-backdrop-filter: blur(5px);
        border: 1px solid rgba(255, 255, 255, 0.3);
        text-align: center;
    }
    .label {
        grid-column: 1 / 4;
        margin-bottom: 1em;
        z-index: 999;
    }
    @media screen and (max-width: 600px) {
        .container {
            padding: 1em;
        }
    }
</style>
