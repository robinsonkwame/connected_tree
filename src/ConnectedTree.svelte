<script>
	import Svelvet from "svelvet";
  import { coreSvelvetStore } from "svelvet/stores/store";

  let startX = -200;
  let startY = -200;
  let offset = 100;

  let yOffset = 50;
  let myref;

  let lastLength = 0;
  let lastEdgeLength = 0;

  const {
      nodesStore,
      nodeIdSelected,
      edgesStore
    } = coreSvelvetStore;
  // from https://github.com/open-source-labs/Svelvet/NPM%20Package/svelvet/Nodes/EditModal.svelte

  let sourceNode;

  $: currentNode = $nodesStore.filter(n => n.id === $nodeIdSelected)[0];

  // retroactively add click handler to new nodes
  // see issue 185
  $: $nodesStore, (() => {
    const currentLength = $nodesStore.length - 1;
    const lastNode = $nodesStore[currentLength]

    if (lastLength < currentLength){
      lastLength = currentLength;
      if(lastNode && !('clickCallback' in lastNode)){
        lastNode['clickCallback'] = handleClick;
        
        // toggle the border
        lastNode['borderRadius'] == undefined ? 
            lastNode['borderRadius'] = 550 : 
              delete lastNode['borderRadius']
        nodesStore.set($nodesStore); // EditModal.svelte:34 ???
      }
    }
  })();

$: $edgesStore, (() => {
  const currentLength = $edgesStore.length - 1;
  const lastEdge = $edgesStore[currentLength]

  if (lastEdgeLength < currentLength){
    if($edgesStore[currentLength].target == undefined){
        $edgesStore[currentLength].targetY = $edgesStore[currentLength].sourceY + yOffset;
    }
    if($edgesStore[currentLength].source == undefined){
        $edgesStore[currentLength].sourceY = $edgesStore[currentLength].targetY - yOffset;
    }
  }
})();

  function introspect(item){
    console.log('\t about to introspect')
    if(item){
      for (var prop in item) {
                console.log(prop + ' = ' + item[prop]);
                if("object" == typeof(item[prop])){
                  for (var prop2 in item[prop]) {
                    console.log(
                      "\t", 
                      prop2 + ' = ' + 
                      ("function" == typeof(item[prop][prop2]) ? '(func)' : item[prop][prop2]));
                  }
                };
            }      
    }
  }

  const toggleBorder = (e) => {
    const currentNode = $nodesStore.filter(n => n.id === $nodeIdSelected)[0]
    'borderRadius' in currentNode ? 
      delete currentNode['borderRadius'] : currentNode['borderRadius'] = 550;
    nodesStore.set($nodesStore); // EditModal.svelte:34 ???    
  }

  const toggleShape = (e) => {
    toggleBorder(e);
    // ... maybe prefix label with Process: <label>?
  }

  const handleClick = (e) => {
    toggleShape(e);
  };

	const initialNodes = [
	  {
	    id: 1,
	    position: { x: startX, y: startY },
	    data: { label: "Input Node" },
	    width: 175,
	    height: 40,
	    bgColor: "white",
      clickCallback: handleClick
	  },
	  {
	    id: 2,
	    position: { x: startX+offset, y: startY+offset },
	    data: { label: "Default Node" },
	    width: 175,
	    height: 40,
	    bgColor: "white",
      borderRadius: 50,
      clickCallback: handleClick
	  }
	];

	const initialEdges = [
	  { id: "e1-2", source: 1, target: 2, label: "edge label" }
	];
</script>

<Svelvet 
  nodes={initialNodes} 
  edges={initialEdges}
  nodeEdit
	nodeLink 
	nodeCreate   
  background
  bind:this={myref}
/>
