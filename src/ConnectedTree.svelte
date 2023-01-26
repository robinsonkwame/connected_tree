<script>
    import Svelvet from "svelvet";

    const initialNodes = [
      {
        id: 1,
        position: { x: 10, y: 10 },
        data: { label: "default styling" },
        width: 150,
        height: 40,
        bgColor: "white"
      }
    ];
</script>

<Svelvet 
height={500}
width={500} 
nodes={initialNodes} 
initialZoom={4} 
initialLocation={initialNodes[0].position} 
nodeLink 
nodeCreate 
nodeEdit
background
shareable
/>
