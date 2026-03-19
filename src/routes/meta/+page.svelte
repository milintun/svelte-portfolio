<script>
    import { base } from '$app/paths';
    import { onMount } from 'svelte';
    import * as d3 from 'd3';
    import BarHorizontal from '../../lib/BarHorizontal.svelte';

    let locData = [];
    let wrangled = [];

    onMount(async () => {
        locData = await d3.csv(`${base}/loc.csv`, row => ({
            ...row,
            line: Number(row.line),
            length: Number(row.length),
            depth: Number(row.depth)
        }));
        wrangled = d3.rollups(
            locData,
            v => d3.sum(v, d => d.length),
            d => d.type
        )
    });


    $: barData = wrangled.map(([type, lines]) => ({ type: String(type), lines: lines }));

</script>

<h1>Meta</h1>

<BarHorizontal data={barData}/>
