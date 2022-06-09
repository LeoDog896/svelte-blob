<script lang="ts">
  interface Position { x: number, y: number };

  const random = (min: number, max: number) => Math.random() * (max - min) + min;
  const transformedPoint = ({ x, y }: Position): Position => ({ x: (size / 3) * x + size / 2, y: (size / 3) * y + size / 2}) 

  export let pointAmount = 6;
  export let size = 100;
  export let debugCircles = false;
  export let debugCircleFill = "pink";
  export let debugCircleRadius = 3;
  export let deformation = 0;
  export let randomProfile: Position[] = []
  export let path = ""

  $: randomProfile = Array(pointAmount).fill(0).map(() => ({ x: random(-deformation, deformation), y: random(-deformation, deformation) }))

  $: points = Array(pointAmount)
    .fill(0).map((_, i) => i * (Math.PI * 2) / pointAmount)
    .map(radius => ({ x: Math.cos(radius), y: Math.sin(radius) }))
  
  $: deformedPoints = points.map(({ x, y }, i) => ({ x: x + randomProfile[i].x, y: y + randomProfile[i].y }))

  $: transformedDeformedPoints = deformedPoints.map(transformedPoint)

  $: path = `M ${transformedDeformedPoints[0].x} ${transformedDeformedPoints[0].y} Q ` + 
    transformedDeformedPoints.slice(1).map(({ x, y }) => `${x} ${y}`).join(", ") +
    ` T ${transformedDeformedPoints[0].x} ${transformedDeformedPoints[0].y}`
</script>
<svg on:click on:mousemove on:mouseover on:mouseleave on:focus on:blur width={size} height={size} >
  {#if debugCircles}
    {#each points as point, i}
      {@const { x, y } = transformedPoint(point)}
      {@const { x: ax, y: ay } = transformedPoint(deformedPoints[i])}
      <circle cx={x} cy={y} r={debugCircleRadius} fill="gray" opacity="0.3"></circle>
      <line x1={x} y1={y} x2={ax} y2={ay} stroke="black"></line>
    {/each}
    {#each deformedPoints as point}
      {@const { x, y } = transformedPoint(point)}
      <circle cx={x} cy={y} r={debugCircleRadius} fill={debugCircleFill}></circle>
    {/each}
  {/if}
  <path fill="transparent" stroke="black" d={path}></path>
</svg>