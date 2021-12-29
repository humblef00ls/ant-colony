<script>
	import P5 from "@macfja/svelte-p5"
import { onMount } from "svelte";
	let w
	let h
	let y = 0;
	let speed = 30;
	let current;
	let ready = false
	let density = 50
	let cheight = 500
	let cwidth = 900
	let hspace = cheight / density
	let wspace = cwidth / density
	let vectors = []

	onMount(() => {
		ready = true
		w = window.innerWidth;
		h = window.innerHeight;
		current = new Date().getTime();

	});
	let sketch = {
	  setup: p5 => {
		p5.createCanvas(cwidth, cheight);
		p5.background(0,0,0);
		 // Set line drawing color to white
		 for(let i =0;i<=hspace;i++){
				for(let j =0;j<= wspace;j++){
					vectors.push( p5.createVector(j*density + p5.random(6) -3 ,i*density + p5.random(6) -3) )
					// p5.line(y,0,j*density ,i*density );
				}
			}
	  },

	  draw: (p5) => {
			p5.frameRate(speed);
			 // Set the background to black
p5.stroke(255,255,255);
			for(let i = 0; i < vectors.length; i++){
				p5.ellipse(vectors[i].x,vectors[i].y,1);

				vectors[i].add(p5.createVector(p5.random(2) -1  ,p5.random(2) - 1, ) )
			}
			y++
			if(y > cwidth ){
				y = 0
			}
			current = p5.frameRate()
	  
			}

	}

	</script>
	<svelte:window bind:innerWidth={w} bind:innerHeight={h} />

	<main>
	{#if ready}
	<label>Speed (frame rate): <input type="range" min="1" max="100" bind:value="{speed}" /> {speed} (Current framerate: {Math.round((current + Number.EPSILON) * 100) / 100})</label>
	<P5 {...sketch} />
	{/if}
</main>
	<style>
		label * {
			vertical-align: middle;
		}
		label{
			height: 20px;
			display: block;
		}
		main{
			display: flex;
			flex-direction: column;
			align-items: center;
			justify-content: center;
			width: 100vw;
			height: 100vh;
		}
	</style>