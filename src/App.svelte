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
		height:100vh;
		display: grid;
		grid-template-columns: 1fr;
		grid-template-rows: 3fr 1fr 2fr;
		gap: 0px 0px;
		grid-template-areas:
		 "map-map"
		 "map-info"
		 "map-nav"
	}

	@media screen and (min-width:600px) {
		#interactiveContainer {
			height: 650px;
			display: grid;
		   grid-template-columns: 1fr 3fr;
		   grid-template-rows: 3fr 1fr;
		   gap: 0px 0px;
		   grid-template-areas:
		    "map-nav map-map"
		    "map-info map-info";
		}
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
		text-align:left;
		background-color: #efefef;
		border:1px solid #999;
		border-radius:2px;
		margin:0 0 0.5rem 0;
		cursor:pointer;
		padding:0.25rem;
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
	}

	span.map-item-title {
		font-size:1.5rem;
		font-weight:bold;
		display:block;
		margin:0.5rem 0 1rem 0;
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
		  zoom={12}
		  centerlnglat={[-71.12, 42.36]}
		  pitch={60}
		  bearing={0}
		  data={dataset}
		  active={active}
	  />
  </div>
</div>
