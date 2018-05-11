---
templateKey: blog-post
title: Add a Simple Google Map to Your Webpage
date: '2013-09-03T11:23:00-07:00'
tags:
  - bootstrap
  - bootstrap google maps
  - bootstrap maps
  - django
  - django google maps
  - google
  - google api
  - google map website
  - google maps
  - google maps api
  - maps api
  - simple geolocating map
  - simple map
---
![null](/img/map-title-300x164.png)

There's just something about having a map of an address that is appealing to people.  Why just list the address like a mailing label when you can place a nice marker on an interactive Google Map?  That's what I thought too when I was adding content to our customer profiles.  I'd never done this before but knew that Google has a great <a title="Google Maps API" href="https://developers.google.com/maps/" target="_blank">API</a> for accessing their data as well as some pretty good examples to follow.  The API covers how to use Google Maps on any device but for the purposes of this post I will be focusing solely on <a title="Web Maps API" href="https://developers.google.com/maps/documentation/javascript/" target="_blank">web</a>.

Let's go over some ground rules for using Google Maps API and then move on to a very simple example and build upon it slightly.  To start off with, you can only make a limited number of requests to Google for maps data before you have to start paying.  For a small company or webpage this limit will not be a problem.

<blockquote>For-profit web sites are permitted to generate up to 25 000 map loads per day using the Google Maps JavaScript API v3. A map load is counted when a map is initialized on a web page. User interaction with a map after it has loaded (eg. panning the map, zooming the map, or changing from roadmap to satellite map) does not have any impact on usage limits.</blockquote>
That's a lot of requests, 25,000, and I know I won't ever get to that point but if you do Google's got your back.  Say one day your site becomes an overnight success.  You're serving tons of traffic and your host is demanding money for the increased CPU usage and storage and you have complaints piling up about site downtimes and slow server times the last thing you want is one more person, Google, coming to complain to you.
<blockquote>In order to ensure that sites which experience short term spikes in usage are not adversely affected by the Maps API usage limits, only sites that exceed the limits for more than 90 consecutive days are subject to the limits.</blockquote>
Nice.  Thanks Google.  After that you better apply for a Google Maps API Business License or purchase additional quota or deal with your high traffic problem and make it go away.  Now, onto our map example.  Google uses straight vanilla JavaScript and in my later example I change a few things to leverage JQuery.
<h2><a title="Simple Example" href="https://developers.google.com/maps/documentation/javascript/examples/map-simple" target="_blank">Simple Map</a></h2>

![null](/img/simplemap-300x132.png)

<pre>&lt;!DOCTYPE html&gt;
&lt;html&gt;
  &lt;head&gt;
    &lt;title&gt;Simple Map&lt;/title&gt;
    &lt;meta name="viewport" content="initial-scale=1.0, user-scalable=no"&gt;
    &lt;meta charset="utf-8"&gt;
    &lt;style&gt;
      html, body, #map-canvas {
        margin: 0;
        padding: 0;
        height: 100%;
      }
    &lt;/style&gt;
    &lt;script src="https://maps.googleapis.com/maps/api/js?v=3.exp&amp;sensor=false"&gt;&lt;/script&gt;
    &lt;script&gt;
var map;
function initialize() {
  var mapOptions = {
    zoom: 8,
    center: new google.maps.LatLng(-34.397, 150.644),
    mapTypeId: google.maps.MapTypeId.ROADMAP
  };
  map = new google.maps.Map(document.getElementById('map-canvas'),
      mapOptions);
}

google.maps.event.addDomListener(window, 'load', initialize);

    &lt;/script&gt;
  &lt;/head&gt;
  &lt;body&gt;
    &lt;div id="map-canvas"&gt;&lt;/div&gt;
  &lt;/body&gt;
&lt;/html&gt;</pre>

This is the example given by Google and it is fairly straight forward.  We have some basic HTML5 page with a map-canvas div.  Since div's have an initial height of 0 the style is set to give it a height.  I had some issues when I tried this exact code and needed to modify it to use specific pixel height.  The specific pixel height is nice because it gives you straight control over how tall it will be.  Applying this with Bootstrap columns will give you a nice width and in my case a widescreen looking map.

The map has a number of options shown above.  Zoom is how zoomed in the map is, the higher the number the closer you are.  Center tells the map where to be centered and uses latitude and longitude, this can be changed dynamically and is what I will show in a second.  MapTypeId refers to the type of map and you can do ROADMAP, SATTELITE, HYBRID, or TERRAIN.  After that we create the map in the DOM and add it to our div.  That's it!  Now this map is kind of simple and boring, right?  It only shows a specific location, no markers, and is hard-coded.  Let's change a bit of that and make, still a basic map, but a geolocating map.

I wanted to create a map that would update to the address of the customer.  I have the address on the screen I was just going to pull it and make a request to Google to center the map on that location and place a marker.  Simple enough.

<h2>Simple Geolocating Map</h2>

![](/img/newmap-300x98.png)

<h3>JavaScript</h3>
<pre><code>$(document).ready(function(){
    //Google Maps
    //Default starting point
    var latlng = new google.maps.LatLng(40.737897, -111.859462);
    var mapOptions = {
        zoom: 1,
        center: latlng,
        mapTypeId: google.maps.MapTypeId.ROADMAP
    }
    //Create our map
    var map = new google.maps.Map(document.getElementById('map-canvas'), mapOptions);
    //Find address
    var address = $("#address").text();
    var geocoder = new google.maps.Geocoder();
    geocoder.geocode( { 'address': address }, function(results, status) {
        if (status == google.maps.GeocoderStatus.OK) {
            map.setCenter(results[0].geometry.location);
            map.setZoom(10);
            var marker = new google.maps.Marker({
                map: map,
                position: results[0].geometry.location,
                url: 'http://maps.google.com/?q=' + $("#address").text()
            });
            google.maps.event.addListener(marker, 'click', function() {
                window.open(marker.url, "_blank");
            });
        }
        else {
            $("#map-message").removeClass("hidden");
        }
    });
});</code></pre>
<h3>HTML</h3>
<pre><code>&lt;div class="col-xs-6 col-sm-6"&gt;
    &lt;address id="address"&gt;
        &lt;strong&gt;{{ customer.name }}&lt;/strong&gt;&lt;br&gt;
        {% if customer.address1 %}
            {{ customer.address1 }}&lt;br&gt;
        {% endif %}
        {% if customer.address2 %}
            {{ customer.address2 }}&lt;br&gt;
        {% endif %}
        {% if customer.address3 %}
            {{ customer.address3 }}&lt;br&gt;
        {% endif %}
        {{ customer.city }}, {{ customer.state_province }} {{ customer.postal_code }}&lt;br&gt;
        {{ customer.country }}&lt;br&gt;
    &lt;/address&gt;
    &lt;a id="map-message" href="{% url "customers.views.customer_edit" customer.customer_number %}" class="hidden btn btn-warning btn-xs"&gt;Google was unable to locate this address. Please verify it.&lt;/a&gt;
    &lt;div id="map-canvas"&gt;&lt;/div&gt;
&lt;/div&gt;</code></pre>
I have used a little bit of JQuery to get the text from my address section and a little bit of Bootstrap to make everything look nice.  I also set the <code>map-canvas</code> to have a height of 150px, but that was personal preference.  <em>It does need a height though or you will not see it.</em>  Going through the JavaScript, I have set an initial starting location as well as set the zoom all the way out.  If the address is not found I want to just show a map of the world.  Create the map and then we need to geolocate the customer's address.  Using Google's geocoder this is easy because they have a variable named address and you can enter anything in there that you would normally enter into a Google Map's search bar.  I simply grab all of the address, including the customer name, and go for the search.  If Google returns with an OK response we focus the map on that location, zoom in a bit, and add a marker to look nice.  I also created a URL to link to Google maps itself when the marker is clicked.  This was a request from some users that wanted to go to a larger map.

I also added a button that lets the user change the address if the customer is not found.  Google will make the attempt and if they can't find it we let the user know that that address may be invalid and to check on it.  If it is right then they can just ignore this problem or report it to Google with the link at the bottom right of the map.

That's it.  There are a lot of different things you can do with the Google Maps API including directions, custom markers, adding more markers, etc.  Take a look at all of the API documentation if you have further needs.  For us this was sufficient and a lot of people like it even if it is just a visually pleasing item and no one will actually use.  Comment below on how you are using the Maps API or if you have any questions on what I've done here.

UPDATE:  If you'd like to read more about Bootstrap compatability with Google Maps check out their <a href="http://getbootstrap.com/getting-started/#third-parties">Third party support</a> page.
