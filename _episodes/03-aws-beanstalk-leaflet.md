---
title: "Leaflet Maps on AWS Elastic Beanstalk"
teaching: 0
exercises: 15
questions:
- "How do I build a Leaflet map?"
objectives:
- "Build a Leaflet map that displays the contents of test.geojson"
keypoints:
- "Linking API calls and data visualization"
- "You get the picture"
---
## ...Continued from [Part 2](/02-elasticbeanstalk-api.html)

**Build your map**

```python

            {%  extends "app/layout.html" %}

            {%  block head %}
                <script src="https://unpkg.com/leaflet@1.0.3/dist/leaflet.js"></script>
                <link rel="stylesheet" href="https://unpkg.com/leaflet@1.0.3/dist/leaflet.css" />
            <style>
                  .leaflet-container { height: 600px; }
                  .legend { text-align: left; line-height: 18px; color: #555; }
                  .legend i { width: 18px; height: 18px; float: left; margin-right: 8px; opacity: 0.7; }
                </style>
            {%  endblock %}

            {% block content %}
                <form id="api-form">
                  <br>
                    <br>
                    <br>
                    Month:<br>
                  <input type="text" name="month" value="e.g. Apr">
                  <br>
                  Variable:<br>
                  <input type="text" name="var" value="e.g. ET">
                  <br><br>
                  <input type="submit" value="Submit">
                <br>
                <br>
                </form>
                <div id="map" height="100%" width="100%"></div>
            {% endblock %}

             {% block script %}
                 // Make marker colors variable
            function getValue(x) {
                return x > 30 ? "#800026" :
                       x > 25 ? "#BD0026" :
                       x > 20 ? "#E31A1C" :
                       x > 15 ? "#FC4E2A" :
                       x > 10 ? "#FD8D3C" :
                       x > 5 ? "#FEB24C" :
                       x > 0 ? "#FED976" :
                           "#FFEDA0";
            }

            function style(feature) {
                return {
                    "color": getValue(feature.properties.val),
                    "radius": 3,
                    "fillOpacity": 0.6,
                    "stroke": false
                };
            }


            // Initialize Leaflet map
                 var map = L.map('map').setView([39, 68], 6);
                 L.tileLayer('https://api.mapbox.com/styles/v1/cloudmaven/civbvg87q001m2ipk2m1qfq5x/tiles/256/{z}/{x}/{y}?access_token=pk.eyJ1IjoiY2xvdWRtYXZlbiIsImEiOiJjaXZidWFzMTEwMGJ1MnlvNnRvOW8xY2lxIn0.zWtZR-VJDOgoxHo5R64VuA', {
                            attribution: 'Map data &copy; <a href="http://openstreetmap.org">OpenStreetMap</a> contributors, <a href="http://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery © <a href="http://mapbox.com">Mapbox</a>',
                        }).addTo(map);

            // Add legend
            var legend = L.control({position: 'bottomright'});
            legend.onAdd = function (map) {

                var div = L.DomUtil.create('div', 'info legend'),
                    grades = [0, 5, 10, 15, 20, 25, 30, 35],
                    labels = [];

                // loop through our density intervals and generate a label with a colored square for each interval
                for (var i = 0; i < grades.length; i++) {
                    div.innerHTML +=
                        '<i style="background:' + getValue(grades[i] + 1) + '"></i> ' +
                        grades[i] + (grades[i + 1] ? '&ndash;' + grades[i + 1] + '<br>' : '+');
                }

                return div;
            };

            legend.addTo(map);

            var month = '{{ month }}';
                 var variable = '{{ var }}';
                 console.log(month);
                 console.log(variable);
                 var xhttp = new XMLHttpRequest();
            xhttp.onreadystatechange = function() {
                if (this.readyState == 4 && this.status == 200) {
                  var geojsonFeature = JSON.parse(this.responseText);
                 L.geoJSON(JSON.parse(geojsonFeature), {pointToLayer: function (feature, latlng) {
                     return L.circleMarker(latlng, style(feature)).bindPopup(feature.properties.val)
                        .on('mouseover', function (e) {
                        this.openPopup()});
                 }}).addTo(map);
                }
            };
            xhttp.open("GET", "/api/simple_chart?month=" + month + "&var=" + variable, true);
            xhttp.send();

            {% endblock %}

```
