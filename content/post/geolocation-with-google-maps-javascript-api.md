+++
title = "Geolocation With Google Maps Javascript Api"
date = "2011-07-22T19:31:13+11:00"
draft = false
categories = ["maps"]
tags = ["JavaScript", "Geolocation"]
+++

In the previous post, we saw how [W3C Geolocation API](http://www.parthbhatt.com/blog/2011/w3c-geolocation-api/) can be used to obtain a user’s location. In this post we will see how the obtained geolocation information can be displayed on a map, using [Google Maps JavaScript API v3](http://code.google.com/apis/maps/documentation/javascript/). We will also see how location information in terms of (latitude, longitude) can be translated into a readable (street) address. This process of obtaining street address from a point location is called [Reverse Geocoding](http://en.wikipedia.org/wiki/Reverse_geocoding).

Google Maps JavaScript API is a JavaScript library that lets you build maps based applications for the web (desktop based browsers and mobile devices).

Note that the intent of this post is not to show how to use the Google Maps JavaScript API. Instead, it is to show how to display the position obtained using the W3C Geolocation API, on a map. For a tutorial on Google Maps JavaScript API, check this out.

First of all, lets see how a map can be displayed on a web page using the Google Maps JavaScript API.

```
    <HTML>
      <HEAD>
        <TITLE>Google Maps JavaScript API Example</TITLE>

        <LINK href="http://code.google.com/apis/maps/documentation/javascript/examples/default.css" rel="stylesheet" type="text/css" />
        <!-- Load the Google Maps JavaScript API v3 -->
        <SCRIPT type="text/javascript" src="http://maps.google.com/maps/api/js?sensor=true"></SCRIPT>

        <SCRIPT type="text/javascript">
          var bangalore = new google.maps.LatLng(12.940322,77.607422);
          var map;

          function initMap()
          {
            var map_options = {
              zoom: 10,
              mapTypeId: google.maps.MapTypeId.ROADMAP
            };
            map = new google.maps.Map(document.getElementById("map_canvas"),
                map_options);

            map.setCenter(bangalore);
            display_text = "<P>Bangalore,<BR/>Karnataka, India</P>" + bangalore;
            showInfoWindow(display_text);
          }

          function showInfoWindow(info_message)
          {
            info_window = new google.maps.InfoWindow();
            info_window.setContent(info_message);
            info_window.setPosition(bangalore);
            info_window.open(map);
          }
        </SCRIPT>
      </HEAD>
      <BODY onload="initMap()">
        <DIV id="map_canvas"> </DIV>
      </BODY>
    </HTML>
```

Following is an extension of the script in the previous post. Besides obtaining the user’s location, it displays the same on a map. It also shows the street address by reverse geocoding on the location returned by W3C Geolocation API.

```
    <HTML>
      <HEAD>
        <TITLE> W3C Geolocation API + Google Maps JavaScript API</TITLE>

        <LINK href="http://code.google.com/apis/maps/documentation/javascript/examples/default.css" rel="stylesheet" type="text/css" />
        <!-- Load the Google Maps JavaScript API v3 -->
        <SCRIPT type="text/javascript" src="http://maps.google.com/maps/api/js?sensor=true"></SCRIPT>

        <SCRIPT type="text/javascript">
          var bangalore = new google.maps.LatLng(12.940322,77.607422);
          var info_window = new google.maps.InfoWindow();
          var initial_location;
          var geo_position;
          var map;

          function getGeolocation()
          {
            var map_options = {
              zoom: 14,
              mapTypeId: google.maps.MapTypeId.ROADMAP
            };
            map = new google.maps.Map(document.getElementById("map_canvas"),
                map_options);

            // Check if the client browser supports W3C Geolocation APIs.
            if(navigator.geolocation)
              navigator.geolocation.getCurrentPosition(handleGeolocationData,
                                                       handleGeolocationError);
            else
              handleGeolocationError(null);
          }

          function handleGeolocationData(position)
          {
            initial_location = new google.maps.LatLng(position.coords.latitude,
                position.coords.longitude);
            map.setCenter(initial_location);
            geo_position = position;
            reverseGeocode();
          }

          function reverseGeocode()
          {
            rev_geocode_req = {'latLng': initial_location};
            geocoder = new google.maps.Geocoder();
            geocoder.geocode(rev_geocode_req, showResults);
          }

          function getResultDescription(result)
          {
            timestamp = new Date(geo_position.timestamp);
            info_text = result.formatted_address + '<BR/>';
            info_text += result.geometry.location.toString() + '<BR/>';
            info_text += 'Accuracy: ' + geo_position.coords.accuracy + ' meters <BR/>';
            info_text += 'Timestamp: ' + timestamp + '<BR/>';

            return info_text;
          }

          function showResults(results, status)
          {
            result_html = "Failed to obtain your street address.";

            if (results)
            {
              if (status == google.maps.GeocoderStatus.OK)
              {
                /*
                for (var i = 0; i < results.length; i++)
                  result_html += getResultDescription(results[i]);
                */
                result_html = getResultDescription(results[0]);
              }
            }
            showInfoWindow(result_html);
          }

          function handleGeolocationError(position_error)
          {
            error_info = "W3C Geolocation Unavailable. <BR/>";
            if (null == position_error)
              error_info += "Geolocation API not supported by your browser.";
            else if (position_error.PERMISSION_DENIED == position_error.code)
              error_info += "PERMISSION_DENIED: Your client is not permitted to share your location.";
            else if (position_error.POSITION_UNAVAILABLE == position_error.code)
              error_info += "POSITION_UNAVAILABLE: Unable to find your location.";
            else if (position_error.TIMEOUT == position_error.code)
              error_info += "TIMEOUT: Timeout while finding your location.";
            else
              error_info += "Unknown error occured while finding your location.";
            error_info += '<BR/>';
            error_info += 'Placing you in Bangalore.';

            initial_location = bangalore;
            map.setCenter(initial_location);
            map.setZoom(10);
            showInfoWindow(error_info);
          }

          function showInfoWindow(info_message)
          {
            info_window.setContent(info_message);
            info_window.setPosition(initial_location);
            info_window.open(map);
          }
        </SCRIPT>
      </HEAD>
      <BODY onload="getGeolocation()">
        <DIV id="map_canvas"> </DIV>
      </BODY>
    </HTML>
```

Click [here](https://www.parthbhatt.com/code/google-maps/w3c_geo_google_maps.html) to see this code in action.
