<script lang="ts">
    import type { VisualizationSpec, View } from "svelte-vega";
    import { Vega } from "svelte-vega";
    import table1 from "../spec/table.json";
    import basic_tree from "../spec/basic_tree.json";
    import tree_data from "../data/tree.json";

    import { writable, readable } from 'svelte/store'
    let spec :  VisualizationSpec = basic_tree;//table1;

    let data = {tree: tree_data}
    let modified_data = {tree: tree_data[0]}

    let view: View;

    let x = writable(0);
    let y = writable(0);

    console.log(spec)

  function introspect(item){
    if(item){
      for (var prop in item) {
          console.log(prop + ' = ' + item[prop]);
          if("object" == typeof(item[prop])){
            for (var prop2 in item[prop]) {
              console.log(
                "\t", 
                prop2 + ' = ' + 
                ("function" == typeof(item[prop][prop2]) ? '(func)' : item[prop][prop2]));
            };
          };
      };
    }
  }

  function setLastItem(event, item){
    if("symbol" == item['mark'].marktype){
      x.set(event.clientX) // left of here!
    }
  }

  function handleClick(event){
    console.log('... we got a click!', event.clientX, event.clientY, '... with ', $x)

    if (event.clientX < $x){
      console.log(event.clientX < $x, "... we'd delete this element!")
      data = modified_data; // and we did?
    }
  }

  function registerViewHandlers() {
      // from ... https://github.com/vega/vega/issues/636
      view.addEventListener("mouseover", function(event, item){
        introspect(item)
        if(item){
          setLastItem(event, item);
        }
      })

      console.log("\t trying to add a mouse click here!")
      view.addEventListener("click", function(event, item){
        handleClick(event);
      });
  }

    $: view ? registerViewHandlers() : "";
  </script>
  
  <main class="content">
    <div class="head">
      <div class="row-content">
        <h3>
          <code>The Tree</code>
        </h3>
      </div>
    </div>

    {$x} {$y}
    <Vega {data} {spec} bind:view={view}/>
  </main>