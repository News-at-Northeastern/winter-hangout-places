<script>
   import { onMount } from "svelte"
   import mapboxgl from 'mapbox-gl'

  let mapRef;

  // props
  export let centerlnglat
  export let zoom


   onMount(async () => {
      mapboxgl.accessToken = 'pk.eyJ1IjoidG1hY2hhZG9udSIsImEiOiJjanVjdDFudDMwMDR4NGRtdGJ4NndiaW9pIn0.JS0ffUQym0L07752GAwMFg';

     // Create the map
      mapRef = new mapboxgl.Map({
         container: 'map',
         style: 'mapbox://styles/tmachadonu/ck1b4p64a07ea1clc4twz44dc/draft',
         center: centerlnglat,
         zoom: zoom
      });

      mapRef.on('load', function() {
         mapRef.addSource('points', {
            'type': 'geojson',
            'data': {
              "features": [
                {
                  "type": "Feature",
                  "properties": {
                    "title": "Curry Student Center Help Desk (across from Crossroads)",
                    "hours": "Mon-Thurs 7am-12am\nFri 7am-11pm\nSat 8am-11pm\nSun 10am-12am",
                    "anchor": "right", "anchor-offset": [-1, 0]
                  },
                  "geometry": {
                    "coordinates": [
                      -71.087592,
                      42.339097
                    ],
                    "type": "Point"
                  },
                  "id": "42e60f1b8fd10671595c74c584ffdd36"
                },
                {
                  "type": "Feature",
                  "properties": {
                    "title": "West Village F Visitor Center",
                    "hours": "Mon-Fri 10am–5pm",
                    "anchor": "right", "anchor-offset": [-1, 0]
                  },
                  "geometry": {
                    "coordinates": [
                      -71.091703,
                      42.337343
                    ],
                    "type": "Point"
                  },
                  "id": "623a5dbc867fc35c8a7da0e0a236973f"
                },
                // {
                //   "type": "Feature",
                //   "properties": {
                //     "title": "Front desk, Marino Center",
                //     "anchor": "right", "anchor-offset": [-1, 0]
                //   },
                //   "geometry": {
                //     "coordinates": [
                //       -71.090376,
                //       42.340156
                //     ],
                //     "type": "Point"
                //   },
                //   "id": "924bd89dbc2528cb745a8b959fece669"
                // },
                // {
                //   "type": "Feature",
                //   "properties": {
                //     "title": "Guard booth, Gainsborough St. bridge",
                //     "anchor": "right", "anchor-offset": [-1, 0]
                //   },
                //   "geometry": {
                //     "coordinates": [
                //       -71.084691,
                //       42.340322
                //     ],
                //     "type": "Point"
                //   },
                //   "id": "e607d931caeba587399ac8134f9f7a99"
                // },
                {
                  "type": "Feature",
                  "properties": {
                    "title": "Guard Booths on the Academic (east) side of campus",
                    "hours": "daily 7am-7pm",
                    "anchor": "left", "anchor-offset": [1, 0]
                  },
                  "geometry": {
                    "coordinates": [
                      -71.08696,
                      42.339069
                    ],
                    "type": "Point"
                  },
                  "id": "f39fff9d63d30344bdf22ae552e1a759"
               },
               {
                 "type": "Feature",
                 "properties": {
                   "title": "Guard Booths on the West Village side of campus",
                   "hours": "daily 7am–11pm",
                   "anchor": "right", "anchor-offset": [-1, 0]
                 },
                 "geometry": {
                   "coordinates": [
                     -71.091264,
                     42.338478
                   ],
                   "type": "Point"
                 },
                 "id": "f39fff9d63d30344bdf22ae552e1a759"
               },
               {
                 "type": "Feature",
                 "properties": {
                   "title": "MasParc Office at the Gainsborough Garage",
                   "hours": "open 24/7",
                   "anchor": "left", "anchor-offset": [1, 0]
                 },
                 "geometry": {
                   "coordinates": [
                     -71.085682,
                     42.340413
                   ],
                   "type": "Point"
                 },
                 "id": "f39fff9d63d30344bdf22ae552e1a759"
               }
              ],
              "type": "FeatureCollection"
            }
         });

         mapRef.addLayer({
            'id': 'points',
            'type': 'circle',
            'source': 'points',
            'paint': {
               'circle-radius': 8,
               'circle-color': 'red',
               'circle-stroke-color': 'black',
               'circle-stroke-width': 2
            }
         });

         mapRef.addLayer({
            'id': 'points-labels',
            'type': 'symbol',
            'source': 'points',
            'layout': {
               'icon-image': 'custom-marker',
               'text-field': [
               'format',
               ['get', 'title'],
               { 'font-scale': 1 },
               '\n',
               {},
               ['get', 'hours'],
               { 'font-scale': 0.7 }
               ],
               'text-font': [
                  'Overpass Bold',
                  'Arial Unicode MS Bold'
               ],
               'text-offset': ['get', 'anchor-offset'],
               'text-anchor': ['get', 'anchor'],
               'text-justify': ['get', 'anchor']
            },
            "paint": {
                "text-color": "#202",
                "text-halo-color": "#fff",
                "text-halo-width": 3
            },
         });

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



   });
</script>



<style>
   #map {
       max-width:90vw;
       width: 900px;
       height: 500px;
     }


</style>

<div id="map"></div>
