<script>
  import { onMount } from 'svelte';

  let canvas;
  let ctx;
  let drawing = false;
  let tool = 'pen';
  let eraserSize = 10;

  const setTool = (selectedTool) => {
    tool = selectedTool;
  };

  const startDrawing = (event) => {
    drawing = true;
    ctx.beginPath();
    ctx.moveTo(event.offsetX, event.offsetY);
  };

  const stopDrawing = () => {
    drawing = false;
    ctx.closePath();
  };

  const draw = (event) => {
    if (!drawing) return;

    if (tool === 'eraser') {
      ctx.lineWidth = eraserSize;
      ctx.strokeStyle = 'white';
    } else {
      ctx.lineWidth = tool === 'pen' ? 50 : 5;
      ctx.strokeStyle = 'black';
    }
    ctx.lineCap = 'round';
    ctx.lineTo(event.offsetX, event.offsetY);
    ctx.stroke();
  };

  onMount(() => {
    ctx = canvas.getContext('2d');
    ctx.fillStyle = 'white';
    ctx.fillRect(0, 0, canvas.width, canvas.height);
  });
</script>

<style>
  .toolbar {
    margin-bottom: 10px;
  }
  button {
    margin-right: 5px;
  }
</style>

<div class="toolbar">
  <button on:click={() => setTool('pen')}>Pen</button>
  <button on:click={() => setTool('pencil')}>Pencil</button>
  <button on:click={() => setTool('eraser')}>Eraser</button>
  <input type="range" min="1" max="50" bind:value={eraserSize} />
</div>
<canvas
  bind:this={canvas}
  width="800"
  height="600"
  on:mousedown={startDrawing}
  on:mouseup={stopDrawing}
  on:mousemove={draw}
></canvas>