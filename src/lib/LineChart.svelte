<script>
    import * as d3 from 'd3';

    export let data = [];

    let width = 1000, height = 300;
    let margin = { top: 10, right: 10, bottom: 30, left: 60 };
    $: usableArea = {
        top: margin.top,
        bottom: height - margin.bottom,
        left: margin.left,
        right: width - margin.right
    };
    $: usableArea.width = usableArea.right - usableArea.left;
    $: usableArea.height = usableArea.bottom - usableArea.top;

    $: xScale = d3.scaleTime()
        .domain(d3.extent(data, d => d.date))
        .range([usableArea.left, usableArea.right]);

    $: yScale = d3.scaleLinear()
        .domain([0, d3.max(data, d => d.count)])
        .range([usableArea.bottom, usableArea.top])
        .nice();

    $: line = d3.line()
        .x(d => xScale(d.date))
        .y(d => yScale(d.count))
        .curve(d3.curveBumpX);

    let xAxisElement, yAxisElement;

    $: {
        d3.select(xAxisElement).call(d3.axisBottom(xScale));
        d3.select(yAxisElement).call(d3.axisLeft(yScale));
    }

    let hoveredDay = null;

    $: dayRegions = (() => {
        if (data.length === 0) return [];
        return data.map((d, i, arr) => {
            const prev = arr[i - 1];
            const next = arr[i + 1];
            const left = prev ? new Date((d.date.getTime() + prev.date.getTime()) / 2) : d.date;
            const right = next ? new Date((d.date.getTime() + next.date.getTime()) / 2) : d.date;
            return {
                date: d.date,
                weekday: d.date.toLocaleString("en", { weekday: "long" }),
                x: xScale(left),
                width: xScale(right) - xScale(left),
                count: d.count
            };
        });
    })();
</script>

<h3>{hoveredDay ? `Lines Edited on ${hoveredDay}s` : 'Lines Edited by Day'}</h3>
<svg viewBox="0 0 {width} {height}" on:mouseleave={() => hoveredDay = null}>
    <g transform="translate(0, {usableArea.bottom})" bind:this={xAxisElement} />
    <g transform="translate({usableArea.left}, 0)" bind:this={yAxisElement} />

    {#each dayRegions as region}
        {#if region.weekday === hoveredDay}
            <rect
                x={region.x}
                y={usableArea.top}
                width={region.width}
                height={usableArea.bottom - usableArea.top}
                fill="var(--color-accent, steelblue)"
                opacity="0.2"
            />
        {/if}
    {/each}

    <path
        d={line(data)}
        fill="none"
        stroke="steelblue"
        stroke-width="2"
    />

    {#each data as d}
        {@const isHighlighted = d.date.toLocaleString("en", { weekday: "long" }) === hoveredDay}
        <circle
            cx={xScale(d.date)}
            cy={yScale(d.count)}
            r={isHighlighted ? 5 : 3}
            fill={isHighlighted ? 'var(--color-accent, steelblue)' : 'steelblue'}
        />
        {#if isHighlighted}
            <text
                x={xScale(d.date)}
                y={yScale(d.count) - 10}
                text-anchor="middle"
                font-size="12"
            >
                {d.count}
            </text>
        {/if}
    {/each}

    {#each dayRegions as region}
        <rect
            x={region.x}
            y={usableArea.top}
            width={region.width}
            height={usableArea.bottom - usableArea.top}
            fill="transparent"
            on:mouseenter={() => hoveredDay = region.weekday}
        />
    {/each}
</svg>

<style>
    svg {
        overflow: visible;
    }
</style>
