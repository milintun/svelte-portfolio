<script>
    import * as d3 from 'd3';

    let width = 400;
    let height = 300;

    export let data = [];
    let margin = { top: 40, right: 150, bottom: 80, left: 80 };
    let innerWidth  = width  - margin.left - margin.right;
    let innerHeight = height - margin.top  - margin.bottom;

    $: yScale = d3.scaleBand()
        .domain(data.map(d => d.type))
        .range([0, innerHeight])
        .padding(0.2);

    $: xScale = d3.scaleLinear()
        .domain([0, d3.max(data, d => d.lines) || 1])
        .range([0, innerWidth]);

    $: colorScale = d3.scaleOrdinal(d3.schemeTableau10)
        .domain(data.map(d => d.type));

    let xAxis, yAxis;
    $: if (xAxis && yAxis) {
        d3.select(xAxis).call(d3.axisBottom(xScale).ticks(5));
        d3.select(yAxis).call(d3.axisLeft(yScale));
    }

    $: maxBar = d3.greatest(data, d => d.lines);

</script>

<div class="container">
    <svg viewBox="0 0 {width} {height}">
        <text
            x={margin.left + innerWidth / 2}
            y={margin.top / 2}
            text-anchor="middle"
            class="chart-title">
            Lines of Code by Language
        </text>
        <g transform="translate({margin.left}, {margin.top + innerHeight})"
           bind:this={xAxis} />
        <text
            x={margin.left + innerWidth / 2}
            y={margin.top + innerHeight + margin.bottom - 10}
            text-anchor="middle"
            class="axis-type">
            Lines of Code
        </text>
        <g transform="translate({margin.left}, {margin.top})"
           bind:this={yAxis} />
        <text
            x={-(margin.top + innerHeight / 2)}
            y={margin.left / 4}
            text-anchor="middle"
            transform="rotate(-90)"
            class="axis-type">
            Programming Language
        </text>
        <g transform="translate({margin.left}, {margin.top})">
            {#each data as d}
                <rect
                    x={0}
                    y={yScale(d.type)}
                    width={xScale(d.lines)}
                    height={yScale.bandwidth()}
                    fill={colorScale(d.type)}
                />
            {/each}
            {#if maxBar}
                <rect
                    x={0}
                    y={yScale(maxBar.type)}
                    width={xScale(maxBar.lines)}
                    height={yScale.bandwidth()}
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                />
                <line
                    x1={xScale(maxBar.lines)}
                    y1={yScale(maxBar.type) + yScale.bandwidth() / 2}
                    x2={xScale(maxBar.lines) + 10}
                    y2={yScale(maxBar.type) + yScale.bandwidth() / 2}
                    stroke="currentColor"
                    stroke-width="1"
                />
                <text
                    x={xScale(maxBar.lines) + 14}
                    y={yScale(maxBar.type) + yScale.bandwidth() / 2}
                    dominant-baseline="middle"
                    class="annotation">
                    Most lines of code
                </text>
            {/if}
        </g>
    </svg>
    <ul class="legend">
        {#each data as d}
            <li style="--color: {colorScale(d.type)}">
                <span class="swatch"></span>
                {d.type} <em>({d.lines})</em>
            </li>
        {/each}
    </ul>
</div>

<style>
    svg {
        max-width: 100%;
        height: auto;
        overflow: visible;
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

    .axis-type {
        font-size: 0.8em;
        fill: currentColor;
    }

    .annotation {
        font-size: 0.7em;
        fill: black;
        font-style: italic;
    }
</style>
