<script>
    import { base } from '$app/paths';
    import { onMount } from 'svelte';
    import * as d3 from 'd3';
    import BarHorizontal from '../../lib/BarHorizontal.svelte';
    import { computePosition, autoPlacement, offset } from '@floating-ui/dom';

    let locData = [];
    let commits = [];
    let margin = { top: 30, right: 20, bottom: 30, left: 30 };
    let xAxis, yAxis;
    let yAxisGridlines;

    let hoveredIndex = -1;
    $: hoveredCommit = commits[hoveredIndex] ?? {};
    let commitTooltip;
    let tooltipPosition = { x: 0, y: 0 };

    let clickedCommits = [];

    async function dotInteraction(index, evt) {
        let hoveredDot = evt.currentTarget;
        if (evt.type === "mouseenter") {
            hoveredIndex = index;
            tooltipPosition = await computePosition(hoveredDot, commitTooltip, {
                strategy: "fixed",
                middleware: [offset(5), autoPlacement()]
            });
        } else if (evt.type === "mouseleave") {
            hoveredIndex = -1;
        } else if (evt.type === "click") {
            if (clickedCommits.includes(commits[index])) {
                clickedCommits = clickedCommits.filter(c => c !== commits[index]);
            } else {
                clickedCommits = [...clickedCommits, commits[index]];
            }
        }
    }

    $: {
        d3.select(yAxisGridlines).call(
            d3.axisLeft(yScale)
              .tickFormat("")
              .tickSize(-usableArea.width)
        );
    }

    $: {
        d3.select(xAxis).call(d3.axisBottom(xScale));
        d3.select(yAxis).call(d3.axisLeft(yScale).tickFormat(d => String(d % 24).padStart(2, "0") + ":00"));
    }

    let width = 1000, height = 600;
    $: [minDate, maxDate] = d3.extent(commits.map(d => d.date));
    $: maxDatePlusOne = new Date(maxDate);
    $: maxDatePlusOne.setDate(maxDatePlusOne.getDate() + 1);
    let usableArea = {
        top: margin.top,
        right: width - margin.right,
        bottom: height - margin.bottom,
        left: margin.left
    };
    usableArea.width = usableArea.right - usableArea.left;
    usableArea.height = usableArea.bottom - usableArea.top;

    $: xScale = d3.scaleTime()
                .domain([minDate, maxDatePlusOne])
                .range([usableArea.left, usableArea.right])
                .nice();

    $: yScale = d3.scaleLinear()
                .domain([0, 24])
                .range([usableArea.bottom, usableArea.top]);

    $: rScale = d3.scaleSqrt()
                .domain(d3.extent(commits, d => d.totalLines))
                .range([5, 30]);

    onMount(async () => {
        locData = await d3.csv(`${base}/loc.csv`, row => ({
            ...row,
            line: Number(row.line),
            depth: Number(row.depth),
            length: Number(row.length),
            date: new Date(row.date + "T00:00" + row.timezone),
            datetime: new Date(row.datetime)
        }));
        commits = d3.groups(locData, d => d.commit).map(([commit, lines]) => {
            let first = lines[0];
            let {author, date, time, timezone, datetime} = first;
            return {
                id: commit,
                url: "https://github.com/vis-society/lab-7/commit/" + commit,
                author, date, time, timezone, datetime,
                hourFrac: datetime.getHours() + datetime.getMinutes() / 60,
                totalLines: lines.length,
                lines: lines
            };
        });
        commits = d3.sort(commits, d => -d.totalLines);
    });

    $: selectedLines = clickedCommits.length > 0
        ? clickedCommits.flatMap(d => d.lines)
        : locData;
    $: selectedCounts = d3.rollup(selectedLines, v => v.length, d => d.type);
    $: allTypes = Array.from(new Set(locData.map(d => d.type)));
    $: barData = allTypes.map(type => ({ label: String(type), value: selectedCounts.get(type) || 0 }));
</script>

<h1>Meta</h1>

<svg viewBox="0 0 {width} {height}">
    <text x={margin.left} y="0" font-size="20" font-weight="bold">Commits by Time of Day</text>
    <g class="gridlines" transform="translate({usableArea.left}, 0)" bind:this={yAxisGridlines} />
    <g transform="translate(0, {usableArea.bottom})" bind:this={xAxis} />
    <g transform="translate({usableArea.left}, 0)" bind:this={yAxis} />
    <g class="dots">
    {#each commits as commit, index }
        <circle
            cx={ xScale(commit.datetime) }
            cy={ yScale(commit.hourFrac) }
            r={ rScale(commit.totalLines) }
            fill="steelblue"
            fill-opacity="0.6"
            role="button"
            tabindex="0"
            aria-label="Commit {commit.id?.slice(0,7)} on {commit.date}"
            class:selected={clickedCommits.includes(commit)}
            on:mouseenter={evt => dotInteraction(index, evt)}
            on:mouseleave={evt => dotInteraction(index, evt)}
            on:click={evt => dotInteraction(index, evt)}
            on:keydown={evt => evt.key === 'Enter' && dotInteraction(index, evt)}
        />
    {/each}
    </g>
</svg>

<dl
    class="info tooltip"
    hidden={hoveredIndex === -1}
    style="top: {tooltipPosition.y}px; left: {tooltipPosition.x}px"
    bind:this={commitTooltip}
>
    <dt>Commit</dt>
    <dd><a href={hoveredCommit.url} target="_blank">{hoveredCommit.id?.slice(0, 7)}</a></dd>

    <dt>Date</dt>
    <dd>{hoveredCommit.datetime?.toLocaleString("en", { dateStyle: "full" })}</dd>

    <dt>Time</dt>
    <dd>{hoveredCommit.time}</dd>

    <dt>Author</dt>
    <dd>{hoveredCommit.author}</dd>

    <dt>Lines edited</dt>
    <dd>{hoveredCommit.totalLines}</dd>
</dl>

<BarHorizontal
    data={barData}
    title={clickedCommits.length > 0 ? "Selected Commits Breakdown" : "Website Breakdown"}
/>

<style>
    svg {
        overflow: visible;
    }

    .gridlines {
        stroke-opacity: .2;
    }

    circle {
        transition: 200ms;
        cursor: pointer;
    }

    circle:hover {
        fill: darkgreen;
    }

    .selected {
        fill: red;
    }

    .info {
        display: grid;
        grid-template-columns: auto 1fr;
        gap: 0.25em 0.75em;
        background: color-mix(in oklch, canvas, transparent 20%);
        box-shadow: 0 2px 8px oklch(0% 0% 0 / 20%);
        border-radius: 6px;
        padding: 0.75em 1em;
        backdrop-filter: blur(4px);
    }

    .tooltip {
        position: fixed;
        top: 1em;
        left: 1em;
        transition-duration: 500ms;
        transition-property: opacity, visibility;
    }

    .tooltip[hidden]:not(:hover, :focus-within) {
        opacity: 0;
        visibility: hidden;
    }

    dt {
        font-weight: bold;
        color: color-mix(in oklch, currentColor, transparent 30%);
    }

    dd {
        margin: 0;
    }
</style>
