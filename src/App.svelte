<script>
	import { onMount } from 'svelte';
	import Map from './charts/Map.svelte'
	import GraphicTitle from './components/GraphicTitle.svelte'
	import GraphicFooter from './components/GraphicFooter.svelte'

	import * as jsondata from '../public/datasets/data.json'
	const dataset = jsondata.default

	export let width = Math.min(
		document.getElementById('interactive').getBoundingClientRect().width,
		1000
	);

	$: active = {
		name: "",
		description: "",
	}

	function handleClick(place) {
		active = place;
		active = active;
	}
</script>

<style>
	#interactiveContainer {
		height:95vh;
		margin-bottom:50vh;
		display: grid;
		grid-template-columns: 55fr 45fr;
		grid-template-rows: 50% 50%;
		gap: 0px 0px;
		grid-template-areas:
		"map-map map-map"
		"map-info map-nav";
	}

	:global(#interactiveContainer div) {
		margin-top: 0;
	}

	.map-info { grid-area: map-info; }
	.map-nav { grid-area: map-nav; }
	.map-map { grid-area: map-map; }

	.map-nav {
		padding-right:0.5rem;
	}

	.map-nav button {
		display:block;
		width: 100%;
		background-color: #efefef;
		border:1px solid #999;
		border-radius:2px;
		margin:0 0.2rem 0.2rem;
		cursor:pointer;
		padding:0.2rem;
		white-space: inherit;
	}

	.map-nav button:hover {
		background-color: #eaeaea;
	}

	.map-nav button:active, .map-nav button:focus {
		background-color: #ddd;
		border:2px solid #999;
	}

	.map-map {
		position: relative;
	  height: 100% !important;
	  width: 100% !important;
	  overflow:hidden;
	}

	span.map-item-title {
		font-size:1.15rem;
		font-weight:bold;
		display:block;
		text-transform:uppercase;
		margin:0.5rem 0 0.5rem 0;
	}

	p.map-item-desc {
		margin-top:0;
		font-size:0.85rem;
	}

	@media screen and (min-width:600px) {
		#interactiveContainer {
			height: 650px;
			margin-bottom:2rem;
			display: grid;
		   grid-template-columns: 25% 75%;
		   grid-template-rows: 3fr 1fr;
		   gap: 0px 0px;
		   grid-template-areas:
		    "map-nav map-map"
		    "map-info map-info";
		}

		span.map-item-title {
			font-size:1.5rem;
		}

		p.map-item-desc {
			font-size:1rem;
		}

		.map-nav button {
			display:block;
			width: 100%;
			text-align:left;
			margin:0 0 0.5rem 0;
			padding:0.25rem;
		}
	}

</style>

<div id="interactiveContainer">
  <div class="map-info">
	  	<span class="map-item-title">{active.name}</span>
  		<p class="map-item-desc">{active.description}</p>
  </div>
  <div class="map-nav">
	  {#each dataset as place}
		  <button
		  		data-place={place.id}
				on:click={() =>
					handleClick(place)
				}
			>
			  {place.name}
		  </button>
	  {/each}
  </div>
  <div class="map-map">
	  <Map
		  zoom={15}
		  centerlnglat={[-71.087433, 42.34061]}
		  pitch={45}
		  bearing={-40}
		  data={dataset}
		  bind:active={active}
	  />
  </div>
</div>
