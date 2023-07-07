<script>

import Statistics from "./Statistics.svelte";
import Timeline from "./Timeline.svelte";
  let showTimeline = false;

  const goToTimeline = () => {
    showTimeline = true;
  };

let hours = 2;
let miles = 0;
let weeklyMiles = 0;
let monthlyMiles = 0;
let yearlyMiles = 0;
let fiveYearMiles = 0;
let lifetimeMiles = 0;
let screenSize = "Standard / Modern models (~6.3)";
let customScreenSize = "";
let scrollingSpeed = "Average (20)";
let zoomLevel = "Normal (1x)";
let currentQuestion = 1;    
let calculated = false;

const screenSizes = {
  "Plus / Max / Ultra models (~6.7)": 6.7,
  "Standard / Modern models (~6.3)": 6.3,
  "Older models (~6.1)":  5.8
};

const swipesPerMinute = {
  "Slow (10)": 10,
  "Average (20)": 20,
  "Fast (30)": 30
};

const zoomLevels = {
  "Small (0.75x)": 0.75,
  "Normal (1x)": 1.0,
  "Large (1.25x)": 1.25
};

const calculateMiles = () => {

    if(currentQuestion < 4) {
    currentQuestion++;
  } else {

  let size = (screenSize === "Custom" && customScreenSize) ? customScreenSize : screenSizes[screenSize];

  if (!size) {
    alert("Please provide a screen size.");
    return;
  }

  let sizeInMiles = size * 0.0000157828; 

  let totalSwipes = hours * 60 * swipesPerMinute[scrollingSpeed];
  miles = totalSwipes * sizeInMiles * zoomLevels[zoomLevel];
  weeklyMiles = miles * 7;
  monthlyMiles = miles * 30; 
  yearlyMiles = miles * 365;
  fiveYearMiles = miles * 365 * 5;
  lifetimeMiles = miles * 365 * 80;

  calculated = true;
    }

};

</script>

<style>
    .question-box {
     margin: 2em auto;
     padding: 2em;
     border: 2px solid #000;
     border-radius: 5px;
     width: 60%;
     background-color: #f9f9f9;
     box-shadow: 0px 0px 10px rgba(0,0,0,0.15);
 }

 .question-box label, .question-box select, .question-box input {
     display: block;
     margin-bottom: 1em;
     width: 100%;
 }

 .question-box button {
     padding: 0.5em 1em;
     font-size: 1em;
     border: none;
     background-color: #007BFF;
     color: #FFF;
     border-radius: 5px;
     cursor: pointer;
 }

 .question-box button:hover {
     background-color: #0056b3;
 }
</style>


<form on:submit|preventDefault={calculateMiles}>
    <div class="question-box">

        {#if currentQuestion >= 1}
            <label for="hours">How many hours per day do you spend on your phone?</label>
            <input id="hours" bind:value={hours} type="number" min="1" />
        {/if}
        
        {#if currentQuestion >= 2}
            <label for="screen">What's your phone's screen size (inches)?</label>
            <select id="screen" bind:value={screenSize}>
                {#each Object.keys(screenSizes) as model (model)}
                    <option>{model}</option>
                {/each}
                <option>Custom</option>
            </select>
            {#if screenSize === 'Custom'}
                <input id="custom" bind:value={customScreenSize} type="number" min="0" />
            {/if}
        {/if}


        {#if currentQuestion >= 3}
            <label for="scrollingSpeed">What's your scrolling speed (scrolls per minute)?</label>
            <select id="scrollingSpeed" bind:value={scrollingSpeed}>
                {#each Object.keys(swipesPerMinute) as speed (speed)}
                    <option>{speed}</option>
                {/each}
            </select>
        {/if}

        {#if currentQuestion >= 4}
            <label for="zoomLevel">What's your phone's zoom level?</label>
            <select id="zoomLevel" bind:value={zoomLevel}>
                {#each Object.keys(zoomLevels) as level (level)}
                    <option>{level}</option>
                {/each}
            </select>
        {/if}

        {#if currentQuestion < 4}
            <button type="submit">Next</button>
        {/if}
        {#if currentQuestion === 4}
            <button type="submit">Calculate</button>
        {/if}
    </div>
</form>

{#if calculated}
    <Statistics 
        miles={miles} 
        weeklyMiles = {weeklyMiles} 
        monthlyMiles={monthlyMiles}
        yearlyMiles = {yearlyMiles} 
        fiveYearMiles={fiveYearMiles} 
        lifetimeMiles={lifetimeMiles}
        goToTimeline={goToTimeline}
    />
{/if}

{#if showTimeline}
    <Timeline 
        miles={miles}
        weeklyMiles={weeklyMiles}
        monthlyMiles={monthlyMiles}
        yearlyMiles={yearlyMiles}
        fiveYearMiles={fiveYearMiles}
        lifetimeMiles={lifetimeMiles} 
    />
{/if}