<script>
    import { onMount } from 'svelte';
    import { fade } from 'svelte/transition';

    export let miles;
    export let weeklyMiles;
    export let monthlyMiles;
    export let yearlyMiles;
    export let fiveYearMiles;
    export let lifetimeMiles;
    export let goToTimeline;

    onMount(() => {
        setTimeout(() => miles = miles, 1000);
        setTimeout(() => weeklyMiles = weeklyMiles, 4000);
        setTimeout(() => monthlyMiles = monthlyMiles, 7000);
        setTimeout(() => yearlyMiles = yearlyMiles, 10000);
        setTimeout(() => fiveYearMiles = fiveYearMiles, 13000);
        setTimeout(() => lifetimeMiles = lifetimeMiles, 16000);
    });
</script>

<div>
    {#if miles}
        <p transition:fade={{ duration: 1000 }}>In a day, your thumb has scrolled approximately <b>{miles.toFixed(2)} miles</b>.</p>
    {/if}
    {#if weeklyMiles}
        <p transition:fade={{ duration: 1000 }}>In a week, this is about <b>{weeklyMiles.toFixed(2)} miles</b> miles.</p>
    {/if}
    {#if monthlyMiles}
        <p transition:fade={{ duration: 1000 }}>In a month: <b>{monthlyMiles.toFixed(2)} miles</b> miles.</p>
    {/if}
    {#if yearlyMiles}
        <p transition:fade={{ duration: 1000 }}>In a year: <b>{yearlyMiles.toFixed(2)} miles</b> miles. </p>
    {/if}
    {#if fiveYearMiles}
        <p transition:fade={{ duration: 1000 }}>In five years: <b>{fiveYearMiles.toFixed(2)} miles</b> miles.</p>
    {/if}
    {#if lifetimeMiles}
        <p transition:fade={{ duration: 1000 }}>In a lifetime (80 years): <b>{lifetimeMiles.toFixed(2)} miles</b> miles.</p>
    {/if}
</div>

<h2>But really, how long is that? Let's take a look.</h2>
<button on:click={goToTimeline}>Next</button>
