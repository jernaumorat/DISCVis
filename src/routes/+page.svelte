<script lang="ts">
  import {isEqual} from 'lodash';

  import Graph from "../Graph.svelte";
  import d3ToPng from 'd3-svg-to-png';

  const rand = () => Math.round(Math.random() * 100)
  let data = [
    // {n: "AS", t: 100, r: 0, b: 0, l: 0},
    // {n: "OP", t: 0, r: 100, b: 0, l: 0},
    // {n: "PA", t: 0, r: 0, b: 100, l: 0},
    // {n: "CO", t: 0, r: 0, b: 0, l: 100},
  ]
  let labels = {
    t: "Assertive",
    r: "Openness",
    b: "Passive",
    l: "Control"
  }
  let corners = {
    tl: 'D',
    tr: 'I',
    br: 'S',
    bl: 'C',
  }

  let [labelOn, cornerOn, axesOn] = [true, true, true]

  // const blankItem = () => ({n: "", t: rand(), r: rand(), b: rand(), l: rand()})
  const blankItem = () => ({n: "", t: 0, r: 0, b: 0, l: 0})
</script>

<Graph bind:data bind:labels bind:corners bind:labelOn bind:cornerOn bind:axesOn/>
<div class="data">
    <div class="buttons">
        <button on:click={() => data = [blankItem(), ...data]}>Add Item</button>
        <button on:click={() => d3ToPng('svg', 'chart', {
                scale: 5,
                format: 'png',
                quality: 1
            })}>Save Image
        </button>
        <div>
            <label for="labelOn">Axis Labels</label>
            <input type="checkbox" id="labelOn" bind:checked={labelOn}/>
        </div>
        <div>
            <label for="cornerOn">Corner Labels</label>
            <input type="checkbox" id="cornerOn" bind:checked={cornerOn}/>
        </div>
        <div>
            <label for="axesOn">Axes</label>
            <input type="checkbox" id="axesOn" bind:checked={axesOn}/>
        </div>
    </div>
    {#each data as item}
        <div class="item">
            <div>
                <label for="initial">Initials</label>
                <input id="initial" type="text" bind:value={item.n}/>
            </div>
            <div>
                <div>
                    <label for="a">{labels.t}</label>
                    <input id="a" type="number" max="100" min="0" bind:value={item.t}/>
                </div>
                <input type="range" max="100" min="0" tabindex="-1" bind:value={item.t}/>
            </div>
            <div>
                <div>
                    <label for="r">{labels.r}</label>
                    <input id="r" type="number" max="100" min="0" bind:value={item.r}/>
                </div>
                <input type="range" max="100" min="0" tabindex="-1" bind:value={item.r}/>
            </div>
            <div>
                <div>
                    <label for="b">{labels.b}</label>
                    <input id="b" type="number" max="100" min="0" bind:value={item.b}/>
                </div>
                <input type="range" max="100" min="0" tabindex="-1" bind:value={item.b}/>
            </div>
            <div>
                <div>
                    <label for="l">{labels.l}</label>
                    <input id="l" type="number" max="100" min="0" bind:value={item.l}/>
                </div>
                <input type="range" max="100" min="0" tabindex="-1" bind:value={item.l}/>
            </div>
            <button on:click={() => {
                data = data.filter(i => !isEqual(i, item))
            }}>Delete</button>
        </div>
    {/each}
</div>

<style>
    :global(body) {
        font-family: sans-serif;
    }

    .buttons {
        display: flex;
        flex-direction: row;
        justify-content: space-evenly;
    }

    .buttons > button {
        width: 10vw;
    }

    .data {
        display: flex;
        flex-direction: column;
        height: 98vh;
        overflow: scroll;
        overflow-x: hidden;
    }

    .item {
        display: flex;
        flex-direction: row;
        justify-content: space-evenly;
        margin: 5px;
        padding: 5px;
    }

    .item > button {
        margin: 0 5px 0 5px
    }

    input {
        width: 2em;
        margin: 0 5px 0 5px;
    }


    input::-webkit-outer-spin-button,
    input::-webkit-inner-spin-button {
        /* display: none; <- Crashes Chrome on hover */
        -webkit-appearance: none;
        margin: 0; /* <-- Apparently some margin are still there even though it's hidden */
    }

    input[type=number] {
        -moz-appearance: textfield; /* Firefox */
    }

    input[type=range] {
        width: 100%;
    }
</style>

