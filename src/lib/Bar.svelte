<script>
    import * as d3 from 'd3';

    let width = 400;
    let height = 300;

    export let data = [];

    let margin = { top: 40, right: 150, bottom: 80, left: 60 };
    let innerWidth  = width  - margin.left - margin.right;
    let innerHeight = height - margin.top  - margin.bottom;

    $: xScale = d3.scaleBand()
        .domain(data.map(d => d.label))
        .range([0, innerWidth])
        .padding(0.2);

    $: yScale = d3.scaleLinear()
        .domain([0, d3.max(data, d => d.value) || 1])
        .range([innerHeight, 0]);

    $: colorScale = d3.scaleOrdinal()
        .domain(data.map(d => d.label))
        .range(d3.quantize(d3.interpolateBlues, data.length + 2).slice(1, -1));

    let xAxis, yAxis;
    $: if (xAxis && yAxis) {
        d3.select(xAxis).call(d3.axisBottom(xScale));
        d3.select(yAxis).call(
            d3.axisLeft(yScale)
                .tickFormat(d => Number.isInteger(d) ? d : "")
                .tickValues(d3.range(0, d3.max(data, d => d.value) + 1))
        );
    }

    $: maxBar = d3.greatest(data, d => d.value);

    $: description = `A bar chart showing the number of projects per year. ${data.map(d => `${d.label}: ${d.value} project${d.value !== 1 ? 's' : ''}`).join(', ')}.`;

    let selectedIndex = -1;
    let liveText = "";
    let showChart = true;

    function toggleBar(index) {
        selectedIndex = selectedIndex === index ? -1 : index;
        if (selectedIndex === -1) {
            liveText = "Selection cleared.";
        } else {
            const d = data[index];
            liveText = `${d.label}: ${d.value} project${d.value !== 1 ? 's' : ''} selected.`;
        }
    }

    function toggleView() {
        showChart = !showChart;
        liveText = showChart ? "Bar chart view shown." : "Table view shown.";
    }
</script>

<p aria-live="polite" class="sr-only">{liveText}</p>

<button
    on:click={toggleView}
    aria-pressed={!showChart}
    class="toggle-button">
    {showChart ? 'Show Table' : 'Show Chart'}
</button>

{#if showChart}
<div class="container">
    <svg viewBox="0 0 {width} {height}" role="img" aria-labelledby="bar-title bar-desc">
        <title id="bar-title">Projects per Year</title>
        <desc id="bar-desc">{description}</desc>
        <text
            x={margin.left + innerWidth / 2}
            y={margin.top / 2}
            text-anchor="middle"
            class="chart-title">
            Projects per Year
        </text>
        <g transform="translate({margin.left}, {margin.top + innerHeight})"
           bind:this={xAxis} />
        <text
            x={margin.left + innerWidth / 2}
            y={margin.top + innerHeight + margin.bottom}
            text-anchor="middle"
            class="axis-label">
            Year
        </text>
        <g transform="translate({margin.left}, {margin.top})"
           bind:this={yAxis} />
        <text
            x={-(margin.top + innerHeight / 2)}
            y={margin.left / 2}
            text-anchor="middle"
            transform="rotate(-90)"
            class="axis-label">
            Number of Projects
        </text>
        <g transform="translate({margin.left}, {margin.top})">
            {#each data as d, index}
                <rect
                    x={xScale(d.label)}
                    y={yScale(d.value)}
                    width={xScale.bandwidth()}
                    height={innerHeight - yScale(d.value)}
                    fill={colorScale(d.label)}
                    opacity={selectedIndex === -1 || selectedIndex === index ? 1 : 0.45}
                    on:click={() => toggleBar(index)}
                    on:keydown={e => { if (e.key === 'Enter') { e.preventDefault(); toggleBar(index); } }}
                    tabindex="0"
                    role="button"
                    aria-label="{d.label}: {d.value} project{d.value !== 1 ? 's' : ''}"
                    aria-pressed={selectedIndex === index}
                />
            {/each}
            {#if maxBar}
                <!-- highlight outline around the tallest bar -->
                <rect
                    x={xScale(maxBar.label)}
                    y={yScale(maxBar.value)}
                    width={xScale.bandwidth()}
                    height={innerHeight - yScale(maxBar.value)}
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                    pointer-events="none"
                />
                <!-- leader line -->
                <line
                    x1={xScale(maxBar.label) + xScale.bandwidth()}
                    y1={yScale(maxBar.value) + (innerHeight - yScale(maxBar.value)) / 2}
                    x2={xScale(maxBar.label) + xScale.bandwidth() + 30}
                    y2={yScale(maxBar.value) + (innerHeight - yScale(maxBar.value)) / 2}
                    stroke="currentColor"
                    stroke-width="1"
                />
                <!-- annotation text at end of leader line -->
                <text
                    x={xScale(maxBar.label) + xScale.bandwidth() + 35}
                    y={yScale(maxBar.value) + (innerHeight - yScale(maxBar.value)) / 2}
                    dominant-baseline="middle"
                    class="annotation">
                    Year with most projects
                </text>
            {/if}
        </g>
    </svg>
    <ul class="legend">
        {#each data as d}
            <li style="--color: {colorScale(d.label)}">
                <span class="swatch"></span>
                {d.label} <em>({d.value})</em>
            </li>
        {/each}
    </ul>
</div>
{:else}
<table class="data-table" aria-label="Table showing project counts by year">
    <caption>Projects by Year</caption>
    <thead>
        <tr>
            <th id="year-header" scope="col">Year</th>
            <th id="projects-header" scope="col">Projects</th>
        </tr>
    </thead>
    <tbody>
        {#each data as d, i}
            <tr>
                <th id="row-{i}" scope="row">{d.label}</th>
                <td aria-labelledby="row-{i} projects-header">{d.value}</td>
            </tr>
        {/each}
    </tbody>
</table>
{/if}

<style>
    rect {
        transition: 300ms;
        outline: none;
    }

    svg {
        max-width: 100%;
        height: auto;
        overflow: visible;
    }

    svg:hover rect:not(:hover), .container:focus-within rect:not(:focus-visible) { opacity: 50%; }
    rect:focus-visible {
        stroke: white;
        stroke-width: 2px;
        stroke-dasharray: 4;
    }

    .container {
        display: flex;
    }

    .legend {
        flex: 1;
    }

    .swatch {
        display: inline-block;
        width: 1em;
        height: 1em;
        background-color: var(--color);
        margin-right: 0.5em;
        flex-shrink: 0;
    }

    li {
        display: flex;
        align-items: center;
    }

    .chart-title {
        font-size: 1em;
        font-weight: bold;
        fill: currentColor;
    }

    .axis-label {
        font-size: 0.8em;
        fill: currentColor;
    }

    .annotation {
        font-size: 0.7em;
        fill: currentColor;
        font-style: italic;
    }

    .sr-only {
        position: absolute;
        left: -9999px;
        width: 1px;
        height: 1px;
        overflow: hidden;
    }

    .toggle-button {
        margin-bottom: 0.75rem;
    }

    .data-table {
        margin-top: 1rem;
        margin-bottom: 1rem;
        border-collapse: collapse;
        width: 100%;
        max-width: 30em;
    }

    .data-table caption {
        font-weight: bold;
        margin-bottom: 0.5em;
        text-align: left;
    }

    .data-table th,
    .data-table td {
        border: 1px solid color-mix(in srgb, currentColor 25%, transparent);
        padding: 0.5em;
        text-align: left;
    }

    .data-table th {
        background-color: color-mix(in srgb, currentColor 8%, transparent);
    }
</style>
