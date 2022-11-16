<script>
    import {onMount} from 'svelte';
    import * as d3 from 'd3';
    import rRect from './rrect.js'

    export let data;
    export let labels;
    export let corners;

    export let labelOn;
    export let cornerOn;
    export let axesOn;

    let vis;
    let svg;
    let ready = false;

    // Use the greater of Assertive/Passive, Openness/Control
    const horizPos = (d) => d.r > d.l ? d.r : 0 - d.l
    const vertPos = (d) => d.t > d.b ? d.t : 0 - d.b

    const draw = (data, labels, labelOn, cornerOn, axesOn) => {
        const padding = 80
        const size = window.innerHeight - (padding * 2)
        const [w, h] = [size, size]

        const xScale = d3.scaleLinear()
        xScale.domain([-100, 100])
        xScale.range([padding, w - padding])
        const yScale = d3.scaleLinear()
        yScale.domain([-100, 100])
        yScale.range([h - padding, padding])

        svg = d3.select(vis)
            .append("svg")
            .attr("width", w)
            .attr("height", h)

        let enteringText = svg.selectAll("text")
            .data(data)
            .enter()
            .append("text")
            .text(d => d.n || " ")
            .attr("x", d => xScale(horizPos(d)))
            .attr("y", d => yScale(vertPos(d)))
            .attr("dy", "0.35em")
            .attr("class", "initial")
            .attr("text-anchor", "middle")

        let enteringRects = svg.selectAll(null)
            .data(enteringText.nodes())
            .enter()
            .append("path")
            .attr("d", node => rRect(node.getBBox().x - 5, node.getBBox().y - 5, node.getBBox().width + 10, node.getBBox().height + 10, 5))
            .attr("class", "initialBox")
            .lower();

        if (axesOn) {
            svg.insert("g")
                .attr("transform", `translate(${[0, h / 2]})`)
                .attr("class", "axis")
                .call(d3.axisBottom(xScale)
                    .tickValues([])
                    .tickSize(10))
                .lower()
            svg.insert("g")
                .attr("transform", `translate(${[w / 2, 0]})`)
                .attr("class", "axis")
                .call(d3.axisRight(yScale)
                    .tickValues([])
                    .tickSize(10))
                .lower()
        }

        if (labelOn) {
            svg.insert("text")
                .text(labels.t)
                .attr("class", "banner top")
                .attr("text-anchor", "middle")
                .attr("dy", "0.35em")
                .attr("x", w / 2)
                .attr("y", padding / 2)
            svg.insert("text")
                .text(labels.r)
                .attr("class", "banner right")
                .attr("text-anchor", "middle")
                .attr("dy", "0.35em")
                .attr("x", w - (padding / 2))
                .attr("y", h / 2)
                .attr("transform", `rotate(90,${w - (padding / 2)},${h / 2})`)
            svg.insert("text")
                .text(labels.b)
                .attr("class", "banner bottom")
                .attr("text-anchor", "middle")
                .attr("dy", "0.35em")
                .attr("x", w / 2)
                .attr("y", h - (padding / 2))
            svg.insert("text")
                .text(labels.l)
                .attr("class", "banner left")
                .attr("text-anchor", "middle")
                .attr("dy", "0.35em")
                .attr("x", padding / 2)
                .attr("y", h / 2)
                .attr("transform", `rotate(270,${padding / 2},${h / 2})`)
        }

        if (cornerOn) {
            svg.insert("text")
                .text(corners.tl)
                .attr("class", "corner topleft")
                .attr("text-anchor", "middle")
                .attr("dy", "0.35em")
                .attr("x", padding / 2)
                .attr("y", padding / 2)
            svg.insert("text")
                .text(corners.tr)
                .attr("class", "corner topright")
                .attr("text-anchor", "middle")
                .attr("dy", "0.35em")
                .attr("x", w - padding / 2)
                .attr("y", padding / 2)
            svg.insert("text")
                .text(corners.br)
                .attr("class", "corner bottomright")
                .attr("text-anchor", "middle")
                .attr("dy", "0.35em")
                .attr("x", w - padding / 2)
                .attr("y", h - padding / 2)
            svg.insert("text")
                .text(corners.bl)
                .attr("class", "corner bottomleft")
                .attr("text-anchor", "middle")
                .attr("dy", "0.35em")
                .attr("x", padding / 2)
                .attr("y", h - padding / 2)
        }
    }

    $: if (ready) {
        if (svg) {
            svg.remove()
        }
        draw(data, labels, labelOn, cornerOn, axesOn)
    }

    onMount(() => ready = true)
</script>

<div bind:this={vis} class="vis"></div>

<style>
    .vis {
        float: right;
    }

    :global(.banner) {
        font-family: "Helvetica Neue";
        font-size: 24px;
    }

    :global(.corner) {
        font-family: "Helvetica Neue";
        font-size: 56px;
        opacity: 0.5;
    }

    :global(.initial) {
        font-family: "Helvetica Neue";
        font-size: 18px;
        white-space: pre;
    }

    :global(.initialBox) {
        fill: ghostwhite;
        stroke: black;
        stroke-width: 2px;
        opacity: 1;
    }

    :global(.topleft) {
        fill: red;
    }

    :global(.topright) {
        fill: #e2e200;
    }

    :global(.bottomright) {
        fill: green;
    }

    :global(.bottomleft) {
        fill: blue;
    }
</style>