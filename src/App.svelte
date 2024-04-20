<script>
  import { onMount } from "svelte";
  let canvas, ctx;
  let isDrawing = false;
  let lineWidth = 15;
  let lineColor = "#000000";
  let isErasing = false;

  onMount(() => {
    ctx = canvas.getContext("2d");
    ctx.lineCap = "round";
    ctx.lineJoin = "round";
    ctx.strokeStyle = lineColor;
    ctx.lineWidth = lineWidth;
  });

  function startDrawing(e) {
    ctx.beginPath();
    ctx.moveTo(
      e.offsetX,
      e.offsetY
    );
    isDrawing = true;
  }

  function endDrawing() {
    ctx.closePath();
    isDrawing = false;
  }

  function draw(e) {
    if (!isDrawing) return;
    ctx.strokeStyle = lineColor;
    ctx.lineWidth = lineWidth;
    ctx.lineTo(e.offsetX, e.offsetY);
    ctx.stroke();
  }

  function toggleEraser() {
    isErasing = !isErasing;
    lineColor = isErasing ? "#FFFFFF" : "#000000"; // Assume the background is white
  }

  function clearCanvas() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
  }
</script>

<div class="App">
  <head>
    <link href="https://fonts.googleapis.com/css2?family=Cute+Font&display=swap" rel="stylesheet">
  </head>
  <h1>Let's Get Painting!</h1>
  <div class="Menu">
    <label>
      Brush Color
      <input type="color"
        bind:value={lineColor}
        on:change={() => isErasing = false}
      />
    </label>
    <label>
      {isErasing ? "Eraser" : "Brush"} Width
      <input type="range" min="1" max="30" bind:value={lineWidth} />
    </label>
    <button on:click={clearCanvas}>
      Clear
    </button>
    <button on:click={toggleEraser}>
      {isErasing ? "Brush" : "Eraser"}
    </button>
  </div>
  <canvas
    bind:this={canvas}
    on:mousedown={startDrawing}
    on:mouseup={endDrawing}
    on:mousemove={draw}
    width="950"
    height="450"
  ></canvas>
</div>
