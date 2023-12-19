<script>
  import opentype from "opentype.js";
  import { onMount } from "svelte";

  let canvas;
  let glyphs = [];

  async function make() {
    const font = await opentype.load("BabelStoneShapes.otf");
    const ctx = await canvas.getContext("2d");

    // Print unicodes glyphs
    for (let i = 0; i < font.numGlyphs; i++) {
      if (+font.glyphs.glyphs[i].advanceWidth === 512) {
        glyphs = [...glyphs, font.glyphs.glyphs[i].unicode];
      }
    }

    // Draw glyphs on canvas
    for (let row = 0; row < 30; row++) {
      for (let column = 0; column < 40; column++) {
        font.glyphs.glyphs[row * 40 + column].draw(
          ctx,
          column * 40,
          row * 40 + 40,
          48
        );
      }
    }
  }

  onMount(() => {
    make();
  });
</script>

<textarea value={JSON.stringify(glyphs.filter((el) => el != null))} />

<main>
  <canvas bind:this={canvas} width={1600} height={1000} />
</main>

<style>
  main {
    background-color: beige;
  }
  canvas {
    background-color: white;
  }
</style>
