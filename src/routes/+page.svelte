<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';
  
    let name = '';
    let pokeCount = 0;
  
    let svg;
    let width = 400;
    let height = 200;
    let margin = { top: 20, right: 30, bottom: 30, left: 40 };
  
    function poke() {
      pokeCount += 1;
      updateChart();
    }
  
    onMount(() => {
      drawChart();
    });
  
    function drawChart() {
      svg = d3.select('#chart')
        .append('svg')
        .attr('width', width)
        .attr('height', height);
  
      updateChart();
    }
  
    function updateChart() {
      svg.selectAll('*').remove(); // Clear previous chart
  
      const data = [pokeCount];
      const xScale = d3.scaleBand()
        .domain(['Poke Count'])
        .range([margin.left, width - margin.right])
        .padding(0.1);
  
      const yScale = d3.scaleLinear()
        .domain([0, d3.max(data, d => d)]).nice()
        .range([height - margin.bottom, margin.top]);
  
      // Draw bars
      svg.selectAll('rect')
        .data(data)
        .enter()
        .append('rect')
        .attr('x', xScale('Poke Count'))
        .attr('y', d => yScale(d))
        .attr('height', d => yScale(0) - yScale(d))
        .attr('width', xScale.bandwidth())
        .attr('fill', 'steelblue');
  
      // Add axes
      svg.append('g')
        .attr('transform', `translate(0,${height - margin.bottom})`)
        .call(d3.axisBottom(xScale));
  
      svg.append('g')
        .attr('transform', `translate(${margin.left},0)`)
        .call(d3.axisLeft(yScale));
    }
  </script>
  
  <style>
    .annoying {
      color: red;
      font-weight: bold;
    }
    main {
      text-align: center;
    }
  </style>
  
  <main>
    <h1>Hello {name ? name : 'there'}!</h1>
  
    <input
      type="text"
      bind:value={name}
      placeholder="Enter your name"
    />
  
    <button on:click={poke}>Poke</button>
  
    {#if pokeCount > 10}
      <p class="annoying">You are annoying</p>
    {/if}
  
    <div id="chart"></div>
  </main>
  