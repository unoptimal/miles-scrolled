<script>
    import * as d3 from "d3";
    import { onMount, afterUpdate } from "svelte";
    import { tweened } from 'svelte/motion';
    import { cubicOut } from 'svelte/easing';

    export let miles;
    export let weeklyMiles;
    export let monthlyMiles;
    export let yearlyMiles
    export let fiveYearMiles;
    export let lifetimeMiles;

    let distanceData = [
    { name: "Daily Scroll", miles: miles },
    { name: "Weekly Scroll", miles: weeklyMiles },
    { name: "Monthly Scroll", miles: monthlyMiles },
    { name: "Yearly Scroll", miles: yearlyMiles },
    { name: "Five Year Scroll", miles: fiveYearMiles },
    { name: "Lifetime Scroll", miles: lifetimeMiles }
  ];


        let landmarks = [
      // { name: "Basketball hoop", miles: 0.00189 }, // 10 feet
      // { name: "Football field", miles: 0.05682 }, // 100 yards
      // { name: "Statue of Liberty", miles: 0.0578 }, // 305 feet
      // { name: "Mount Everest", miles: 0.134 }, //36,070 feet
      // { name: "Marathon", miles: 26.2 },
      // { name: "Grand Canyon", miles: 277 }, 
      { name: "Length of the US-Mexico border", miles: 1954 },
      { name: "Route 66", miles: 2448 },
      { name: "Length of the Amazon River", miles: 4345 },
      { name: "Circumference of the Earth", miles: 24901 }
    ];

    let colors = ['green', 'blue', 'red', 'purple', 'orange', 'yellow'];
    
  let svg;
  let yScale;

  const drawLandmarks = () => {
    const xPosition = svg.node().getBoundingClientRect().width / 2; 

    svg.selectAll('.landmark-circle')
      .data(landmarks)
      .enter()
      .append('circle')
      .attr('class', 'landmark-circle')
      .attr('cx', xPosition)
      .attr('cy', d => yScale(d.miles))
      .attr('r', 5)
      .attr('fill', 'red');

      svg.selectAll('.landmark-text')
      .data(landmarks)
      .enter()
      .append('text')
      .attr('class', 'landmark-text')
      .attr('x', xPosition + 10)
      .attr('y', d => yScale(d.miles))
      .text(d => d.name)
      .attr("text-anchor", "start")
      .attr("dominant-baseline", "middle")
      .attr("font-size", "12px");
  };

  const drawDistanceMarkers = () => {
    const xPosition = svg.node().getBoundingClientRect().width / 2; 
    const markers = [100, 200, 300, 400, 500, 600, 700, 800, 900, 1000, 1500, 2000, 2500, 3000, 3500, 4000, 4500, 5000, 5500, 6000, 6500, 7000, 7500, 8000, 8500, 9000, 9500, 10000, 11000, 12000, 13000, 14000, 15000, 16000, 17000, 18000, 19000, 20000, 21000, 22000, 23000, 24000, 25000];

    svg.selectAll('.marker-line')
      .data(markers)
      .enter()
      .append('line')
      .attr('class', 'marker-line')
      .attr('x1', xPosition - 5)
      .attr('y1', d => yScale(d))
      .attr('x2', xPosition + 5)
      .attr('y2', d => yScale(d))
      .attr('stroke', 'black')
      .attr('stroke-width', 1);

      svg.selectAll('.marker-text')
      .data(markers)
      .enter()
      .append('text')
      .attr('class', 'marker-text')
      .attr('x', xPosition + 10)
      .attr('y', d => yScale(d))
      .text(d => d)
      .attr("text-anchor", "start")
      .attr("dominant-baseline", "middle")
      .attr("font-size", "12px");
  };


const getLineXPosition = (centerX, index) => {
        const offset = 100;  
        return centerX + index * offset;
    };

const drawUserLines = () => {
    svg.selectAll(".user-line").remove();

    const centerXPosition = svg.node().getBoundingClientRect().width / 2; // Centers the lines

    distanceData.forEach((datum, index) => {

    let xPosition = getLineXPosition(centerXPosition, index);

      let path = svg.append('path')
        .datum([{x: xPosition, y: yScale(0)}, {x: xPosition, y: yScale(datum.miles)}])
        .attr('class', 'user-line')
        .attr('d', d3.line()
          .x(function(d) { return d.x; })
          .y(function(d) { return d.y; })
        )
        .attr('stroke', colors[index]) 
        .attr('stroke-width', 2)
        .attr('fill', 'none');

      const lineLength = path.node().getTotalLength();
      path.attr('stroke-dasharray', lineLength);
    });
  };

  onMount(() => {
    svg = d3.select('#timeline')
      .append('svg')
      .attr('width', 1000)
      .attr('height', 8000);

    yScale = d3.scaleLinear()
      .domain([0, d3.max(landmarks, d => d.miles) + 200])
      .range([0, 8000]);

    drawLandmarks();
    drawDistanceMarkers();

    window.addEventListener('scroll', () => {
      let userLine = svg.select('.user-line').node();
    // animation goes here
    });
  });

  afterUpdate(() => {
     drawUserLines();

  });


</script>

<div id="timeline"></div>

<style>
  #timeline {
      width: 100%;
      display: flex;
      justify-content: center;
      align-items: center;
  }
</style>
