<script>
   import { onMount, afterUpdate } from "svelte"
   import mapboxgl from 'mapbox-gl'

  let mapRef;

  // props
  export let centerlnglat
  export let pitch
  export let bearing
  export let zoom
  export let data
  export let active

  let datapoints = []


   onMount(async () => {
      data.forEach(d => {
         let newobj = {
           "type": "Feature",
           "properties": {
             "title": d.name,
             "anchor": "right",
             "anchor-offset": [-1, 0]
           },
           "geometry": {
             "coordinates": [
               d.lng,
               d.lat
             ],
             "type": "Point"
           },
           "id": d.id
         }

         datapoints.push(newobj)

      })

      mapboxgl.accessToken = 'pk.eyJ1IjoidG1hY2hhZG9udSIsImEiOiJjanVjdDFudDMwMDR4NGRtdGJ4NndiaW9pIn0.JS0ffUQym0L07752GAwMFg';

     // Create the map
      mapRef = new mapboxgl.Map({
         container: 'map',
         style: 'mapbox://styles/tmachadonu/ckgic5tcc7iur19mynsutt5jh',
         center: centerlnglat,
         zoom: zoom,
         bearing: bearing,
         pitch: pitch
      });

      mapRef.on('load', function() {
         mapRef.addSource('points', {
            'type': 'geojson',
            'data': {
              "features": datapoints,
              "type": "FeatureCollection"
            }
         });

         // mapRef.addLayer({
         //    'id': 'points-circles',
         //    'source': 'points',
         //    'type': 'circle',
         //    'paint': {
         //       'circle-radius': 4,
         //       'circle-color': 'white',
         //       'circle-stroke-color': 'red',
         //       'circle-stroke-width': 4
         //    }
         // });

         datapoints.forEach(function(marker) {
            let el = document.createElement('div');
            el.className = 'marker';
            el.dataset.placeid = marker.id;
            el.style.backgroundImage = "url('/photos/" + marker.id + ".jpg')";
            // el.style.backgroundImage = "url('//news.northeastern.edu/interactive/2019/09/fall-2019-campus-construction/photos/" + marker.id + ".jpg')";

            el.addEventListener('click', function() {
               active = data.filter(d => (d.id === marker.id))[0];
               active = active
               activeChange(active)
            })

            new mapboxgl.Marker(el)
             .setLngLat(marker.geometry.coordinates)
             .addTo(mapRef);
         })

         // mapRef.addLayer({
         //    'id': 'points-labels',
         //    'source': 'points',
         //    'type': 'symbol',
         //    'layout': {
         //       'icon-image': 'custom-marker',
         //       'text-field': [
         //       'format',
         //       ['get', 'title'],
         //       { 'font-scale': 1 }
         //       ],
         //       'text-font': [
         //          'DIN Offc Pro Medium',
         //          'Arial Unicode MS Bold'
         //       ],
         //       'text-offset': ['get', 'anchor-offset'],
         //       'text-anchor': ['get', 'anchor'],
         //       'text-justify': ['get', 'anchor']
         //    },
         //    "paint": {
         //        "text-color": "#202",
         //        "text-halo-color": "#fff",
         //        "text-halo-width": 3
         //    },
         // });

         // mapRef.on('click', 'points', function(e) {
         //    var coordinates = e.features[0].geometry.coordinates.slice();
         //    var description = e.features[0].properties.title;
         //
         //    // Ensure that if the map is zoomed out such that multiple
         //    // copies of the feature are visible, the popup appears
         //    // over the copy being pointed to.
         //    while (Math.abs(e.lngLat.lng - coordinates[0]) > 180) {
         //       coordinates[0] += e.lngLat.lng > coordinates[0] ? 360 : -360;
         //    }
         //
         //    new mapboxgl.Popup()
         //       .setLngLat(coordinates)
         //       .setHTML(description)
         //       .addTo(mapRef);
         // });
         //
         // // Change the cursor to a pointer when the mouse is over the places layer.
         // mapRef.on('mouseenter', 'places', function() {
         //    mapRef.getCanvas().style.cursor = 'pointer';
         // });
         //
         // // Change it back to a pointer when it leaves.
         // mapRef.on('mouseleave', 'places', function() {
         //    mapRef.getCanvas().style.cursor = '';
         // });

      })
   }); //onMount

   afterUpdate(() =>
      activeChange(active)
   )

   function activeChange(active) {
      let prevactive = document.querySelector(".active");
      let activeplace = document.querySelector("[data-placeid='" + active.id + "']");
      if (prevactive) {
         prevactive.classList.remove("active")
      }
      if (activeplace) {
         activeplace.classList.add("active")
      }

      if (active.lng) {
         mapRef.flyTo({
            center: [
               active.lng,
               active.lat
            ],
            zoom: 18,
            essential: true
         });
      }
	}
</script>



<style>
   :global(.marker) {
     background-size:cover;
     border:2px solid black;
     width: 75px;
     height: 75px;
     border-radius: 50%;
     cursor: pointer;
     position: absolute;
      top: -36px;
      left: 0;
      will-change: transform;
     z-index: 1
   }

   :global(.marker.active) {
     width: 300px;
     height: 300px;
     top: -150px;
     z-index:50
   }

   :global(.marker::after) {
     content: " ";
     position: absolute;
     top: 100%; /* At the bottom of the tooltip */
     left: 50%;
     margin-left: -10px;
     border-width: 10px;
     border-style: solid;
     border-color: black transparent transparent transparent;
   }

   :global(.mapboxgl-control-container div) {
      display:inline-block;
   }

   :global(.mapboxgl-control-container) {
      display:inline-block;
      position:relative;
      top:-25px;
      background-color:rgba(255, 255, 255, 0.8);
   }

   :global(.mapboxgl-control-container a) {
      color:#555;
      font-size:10px;
      margin-right:0.5rem;
   }

   #map {

      position: absolute;
      top: 0;
      bottom: 0;
      width: 100%;
  }

</style>

<div id="map"></div>
