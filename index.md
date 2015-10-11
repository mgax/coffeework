---
layout: base
---
<link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/leaflet/0.7.5/leaflet.css">
<style>
  html, body { height: 100%; margin: 0 }
  ul { margin: 0 }
  #map { position: absolute; top: 0; bottom: 0; left: 0; right: 0 }
  .leaflet-container .leaflet-control-attribution a { color: #557b8a }
  .place {
    border-left: 10px solid transparent;
    border-right: 10px solid transparent;
    border-top: 10px solid #ccc;
  }
  .place p {
    background: #ccc;
    overflow: hidden; position: absolute; left: -10px; bottom: 10px; margin: 0;
    padding: 2px 5px; font-size: 16px;
  }
  .place p a { display: block; color: #5a91a7; text-decoration: none }
  .place p a:hover { color: #7dabbd }
</style>

<div id="map"></div>

<script src="//cdnjs.cloudflare.com/ajax/libs/leaflet/0.7.5/leaflet.js"></script>
<script src="//cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.23/browser.min.js"></script>
<script>
var map = L.map('map', {
  center: [44.433219, 26.095353],
  zoom: 12,
})

L.tileLayer('http://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}.png', {
  attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, &copy; <a href="http://cartodb.com/attributions">CartoDB</a>'
}).addTo(map)

var data = {
  'type': 'FeatureCollection',
  'features': [
    {% for page in site.pages %}
      {% if page.brand %}
        {
          type: 'Feature',
          geometry: {
            type: 'Point',
            coordinates: [{{ page.coordinates[1] }}, {{ page.coordinates[0] }}],
          },
          properties: {
            url: '{{ site.base_url }}{{ page.url | split:".html" }}',
            brand: '{{ page.brand }}',
          }
        },
      {% endif %}
    {% endfor %}
  ],
}

L.geoJson(data, {
  pointToLayer: function(feature, latLng) {
    var myIcon = L.divIcon({
      html: '<p><a href="' + feature.properties.url + '">' + feature.properties.brand + '</a></p>',
      className: 'place',
      iconSize: [0, 0],
    })
    return L.marker(latLng, {icon: myIcon})
  },
}).addTo(map)
</script>
