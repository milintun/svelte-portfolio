<script>
    import * as d3 from 'd3';

    let width = 400;
    let height = 200;

    export let data = [];
    export let title = "";
    let margin = { top: 40, right: 80, bottom: 60, left: 80 };
    let innerWidth  = width  - margin.left - margin.right;
    let innerHeight = height - margin.top  - margin.bottom;

    $: yScale = d3.scaleBand()
        .domain(data.map(d => d.label))
        .range([0, innerHeight])
        .padding(0.2);

    $: xScale = d3.scaleLinear()
        .domain([0, d3.max(data, d => d.value) || 1])
        .range([0, innerWidth]);

    $: colorScale = d3.scaleOrdinal(d3.schemeTableau10)
        .domain(data.map(d => d.label));

    let xAxis, yAxis;
    $: if (xAxis && yAxis) {
        d3.select(xAxis).call(
            d3.axisBottom(xScale)
                .ticks(Math.max(1, Math.min(d3.max(data, d => d.value) || 1, 10)))
                .tickFormat(d3.format("d"))
        );
        d3.select(yAxis).call(d3.axisLeft(yScale));
    }

    $: maxBar = d3.greatest(data, d => d.value);

</script>

<div class="container">
    <svg viewBox="0 0 {width} {height}">
        <text
            x={margin.left + innerWidth / 2}
            y={margin.top / 2}
            text-anchor="middle"
            class="chart-title">
            {title || "Lines of Code by Language"}
        </text>
        <g transform="translate({margin.left}, {margin.top + innerHeight})"
           bind:this={xAxis} />
        <text
            x={margin.left + innerWidth / 2}
            y={margin.top + innerHeight + margin.bottom - 10}
            text-anchor="middle"
            class="axis-label">
            Lines of Code
        </text>
        <g transform="translate({margin.left}, {margin.top})"
           bind:this={yAxis} />
        <text
            x={-(margin.top + innerHeight / 2)}
            y={margin.left / 4}
            text-anchor="middle"
            transform="rotate(-90)"
            class="axis-label">
            Language
        </text>
        <g transform="translate({margin.left}, {margin.top})">
            {#each data as d}
                <rect
                    x={0}
                    y={yScale(d.label)}
                    width={xScale(d.value)}
                    height={yScale.bandwidth()}
                    fill={colorScale(d.label)}
                />
            {/each}
            {#if maxBar}
                <rect
                    x={0}
                    y={yScale(maxBar.label)}
                    width={xScale(maxBar.value)}
                    height={yScale.bandwidth()}
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                />
                <text
                    x={xScale(maxBar.value) + 6}
                    y={yScale(maxBar.label) + yScale.bandwidth() / 2}
                    dominant-baseline="middle"
                    text-anchor="start"
                    class="annotation">
                    Most lines
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
        font-size: 0.9em;
        font-weight: bold;
        fill: currentColor;
    }

    .axis-label {
        font-size: 0.7em;
        fill: currentColor;
    }

    .annotation {
        font-size: 0.65em;
        fill: currentColor;
        font-style: italic;
    }
</style>
