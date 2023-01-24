<script lang="ts">
    import type { VisualizationSpec } from "svelte-vega";
    import { Vega } from "svelte-vega";
    import data1 from "../data/data1.json";
    import table1 from "../spec/table.json";
    import flare from "../data/flare.json"
    import tree from "../spec/tree.json"

    let vega_spec = tree
    vega_spec['data']['values'] = flare; // since file://../data/flare.json is weird

    // note: in this example we're specifying vega, not vega-lite
    let spec :  VisualizationSpec = table1;//table1;

    console.log(spec)


    let info = "";
    // NEED TO UNDERSTAND HOW data is used in Svelte-Vega
    //  Then try to plot a basic simple Tree
    //
    //  Then try to plot the more complex flare; if that works
    // ?try swapping in the fancy file folder looking on
    let data = data1;

    const handlers: {
        tooltip: (...args: unknown[]) => void;
    } = { tooltip: (...args) => (info = JSON.stringify(args)) };    
    
    function handleUpdateData() {
      const table = [];
      for (let i = 1; i <= 20; i++) {
        table.push({
          amount: Math.round(Math.random() * 100),
          category: String.fromCharCode(65 + i),
        });
      }
      data = { table };
    }
  </script>
  
  <main class="content">
    <div class="head">
      <div class="row-content">
        <h3>
          <code>&lt;VegaLite&gt;</code> Svelte Component
        </h3>
        <button on:click={handleUpdateData} class="common-button"
          >Update Data</button
        >
      </div>
    </div>

    Will recompile when spec changes and update when data changes.

    <!-- <VegaLite {data} {spec} signalListeners={handlers}/> -->
    <!-- <Vega {spec} signalListeners={handlers}/> works -->

    <div>
        Hover info: <code>{info}</code>
      </div>    
  </main>
  
  <style>
    .content {
      display: flex;
      flex-direction: column;
      font-family: "SF Pro Text", "Myriad Set Pro", "SF Pro Icons",
        "Helvetica Neue", Helvetica, Arial, sans-serif;
    }
  
    .row-content {
      display: flex;
      align-items: center;
    }
  
    .head {
      display: flex;
      align-items: center;
      justify-content: space-between;
    }
  
    .common-button {
      display: flex;
      align-items: center;
      height: 2em;
      margin-left: 0.5em;
      padding: 0.5em;
      border: 1px solid #5382b0;
      border-radius: 5px;
    }
  
    .common-button:active {
      background-color: #cce6ff;
    }

    button {
      font-family: inherit; /* 1 */
      font-size: 100%; /* 1 */
      line-height: 1.15; /* 1 */
      margin: 0; /* 2 */
      background-color: transparent;
      text-transform: none;
      border: none;
      overflow: visible;
      -webkit-appearance: button;
    }
  
    button:active {
      color: inherit;
    }
  </style>