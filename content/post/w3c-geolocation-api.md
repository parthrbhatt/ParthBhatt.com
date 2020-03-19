+++
title = "W3c Geolocation Api"
image = ""
date = 2011-06-22T19:30:09+11:00
draft = false
categories = ["maps"]
tags = ["JavaScript", "Geolocation"]
+++

I've always liked the idea of a service that provides information keeping in mind the location from which it is requested. That certainly is a way to fill the user with information that is most relevant. For example, consider an application that shows all Chinese restaurants around you as opposed to an app that just shows you all Chinese restaurants. Or may be an app that says how far you are from the nearest KFC! Or a photo sharing site that can show you photos taken around you (yes, I am talking about [Flickr](http://www.flickr.com/map)).

But, how would these services work? How could such a service identify where the user is located? If the idea of finding answers to these questions fascinates you, read on!

Short answer to these questions is W3C Geolocation APIs.

W3C geolocation API helps you build applications that are location-aware, by providing a standardized way of programmatically obtaining a user’s location information from a client-side application. This information can be shared with trusted web sites, so that these websites can provide services keeping in mind where the user is located. So that you, as a user, are furnished with information that is most relevant to you.

To obtain the user’s location, the client (browser) contacts a location information server (e.g. Google Location Services) and the user’s location is determined on the basis of information like visible access points (their location and signal strength), your IP address, getting your location from a GPS device, GSM/CSMA cell ids, Bluetooth MAC address, RFID etc.

Note that the geolocation APIs, however, are totally agnostic to the way the client obtains the user’s location. Therefore, an application that makes use of the geolocation API, does not have to worry about the intricacies of how the user’s location would be obtained. In other words, the underlying client implementation (possibly, a web browser) takes care of actually figuring out the user’s physical location and providing it to the consumer of the APIs (the client side script using the geolocation APIs).

The W3C Geolocation API spec can be found here. The rest of this article focuses on how these APIs can be used.

The code snippet below uses the geolocation APIs to identify the user’s location:

```
<HTML>
  <HEAD>
    <TITLE>W3C Geolocation Example</TITLE>
    <SCRIPT type="text/javascript">
    function getGeolocation()
    {
      // Check if the client browser supports W3C geocoding.
      if(navigator.geolocation)
        navigator.geolocation.getCurrentPosition(handleGeolocationData,
                                                 handleGeolocationError);
    }

    /* Success callback for navigator.geolocation.getCurrentPosition()
     */
    function handleGeolocationData(position)
    {
      latitude = position.coords.latitude;
      longitude = position.coords.longitude;

      alert ("Your location: (" + latitude + ", " + longitude + ")");
    }

    /* Failure callback for navigator.geolocation.getCurrentPosition()
     */
    function handleGeolocationError(position_error)
    {
      alert ("Unable to find your location.");
    }
    </SCRIPT>
  </HEAD>
  <BODY onload="getGeolocation()">
  </BODY>
</HTML>
```

Click here to see this code snippet in action. Note that, as you already know, this page will determine your location, which is not saved anywhere on this site.

Of course, this script needs better error handling. Specifically, when geolocation fails, it can’t identify why it failed. But we’ll look into that bit in a while. For now lets focus on what it does rather than what it does not do.

If you save this in an html page and open it in your web browser, you should see a prompt asking you if you wish to share your location information (unless your browser does not support W3C geolocation API). The prompt will look something like this:

![firefox](../../media/screenshot_ff3.jpg)

Unless you click on `Share Location`, you will not see your location.

Lines 9-10 in the snippet above use getCurrentLocation(), which is an asynchronous API. We pass two callback functions to getCurrentLocation(). The first, handleGeolocationData(), would get called if geolocation data is available and the other, handleGeolocationError(), would get called in case when geolocation data is not available. The call to getCurrentLocation() returns immediately and then one of the callback functions is called depending upon the fate of the browser’s attempt to identify the user’s location.

As you might have noticed, the success callback is called with a parameter. This parameter is an object of type Position. It contains all the available info about the user’s location.

The failure callback is called with an object of type [PositionError](http://dev.w3.org/geo/api/spec-source.html#position_error_interface). This helps the failure callback function to identify what went wrong.

Lets improve this script a bit and try to make it a little more robust.

```
<HTML>
  <HEAD>
    <TITLE>W3C Geolocation Example</TITLE>
    <SCRIPT type="text/javascript">
    function getGeolocation()
    {
      // Check if the client browser supports W3C Geolocation APIs.
      if(navigator.geolocation)
        navigator.geolocation.getCurrentPosition(handleGeolocationData,
                                                 handleGeolocationError);
      else
        handleGeolocationError(null);
    }

    function handleGeolocationData(position)
    {
      // The timestamp is in a DOMTimeStamp object, which is milliseconds
      // since the start of the Unix Epoch. So we need to convert it.
      timestamp = new Date(position.timestamp);

      location_info = "Your Geolocation Information: <BR/><BR/>";
      location_info += "Timestamp: " + timestamp.toLocaleString() + "<BR/>";
      location_info += "Latitude and Longitude: (" + position.coords.latitude + ", " + position.coords.longitude + ") <BR/>";
      location_info += "Accuracy: " + position.coords.accuracy + " meters<BR/>";

      // Check if any of the optional attributes of position.coords is specified.
      if (position.coords.altitude)
        location_info += "Altitude: " + position.coords.altitude + "<BR/>";
      if (position.coords.altitudeAccuracy)
        location_info += "Altitude Accuracy: " + position.coords.altitudeAccuracy + "<BR/>";
      if (position.coords.heading)
        location_info += "Heading: " + position.coords.heading + "<BR/>";
      if (position.coords.speed)
        location_info += "Speed: " + position.coords.speed + "<BR/>";

      document.getElementById("location").innerHTML = location_info;
    }

    function handleGeolocationError(position_error)
    {
      error_info = "W3C Geolocation Unavailable. <BR/>";
      if (null == position_error)
        error_info += "Geolocation API not supported by your browser.";
      else if (position_error.PERMISSION_DENIED == position_error.code)
        error_info += "PERMISSION_DENIED: Your client is not permitted to share your location";
      else if (position_error.POSITION_UNAVAILABLE == position_error.code)
        error_info += "POSITION_UNAVAILABLE: Unable to find your location.";
      else if (position_error.TIMEOUT == position_error.code)
        error_info += "TIMEOUT: Timeout while finding your location.";
      else
        error_info += "Unknown error occured while finding your location.";

      document.getElementById("location").innerHTML = error_info;
    }
    </SCRIPT>
  </HEAD>
  <BODY onload="getGeolocation()">
    <DIV id="location"> </DIV>
  </BODY>
</HTML>
```

The code above displays all the location information that is made available to the success callback function. It also does a better job at error handling, identifying why geolocation failed.

To see how this example works, click [here](https://www.parthbhatt.com/code/w3c-geo/w3c_geo_getCurrentPosition.html).

While, getCurrentPosition() helps get the location of the user in a one-shot manner, the geolocation API offers another function, watchPosition() that lets the application to monitor the position of a user. watchPosition(), once invoked, calls the success callback function every time a user’s location changes. Following is a snippet that shows use of watchPosition():

```
<HTML>
  <HEAD>
    <TITLE>W3C Geolocation Example</TITLE>
    <SCRIPT type="text/javascript">
      var watch_id;

      function initGeolocation()
      {
        // Check if the client browser supports W3C Geolocation APIs.
        if(navigator.geolocation)
          watch_id = navigator.geolocation.watchPosition(handleGeolocationData,
                                                         handleGeolocationError);
        else
          handleGeolocationError(null);
      }

      function unInitGeolocation()
      {
        // Clear the watch if we initialized it.
        if(navigator.geolocation)
          navigator.geolocation.clearWatch(watch_id);
      }

      /* The callbacks handleGeolocationData() and
       * handleGeolocationError() remain the same
       * and need to be added here. */
    </SCRIPT>
  </HEAD>
  <BODY onload="initGeolocation()" onunload="unInitGeolocation()">
    <DIV id="location"> </DIV>
  </BODY>
</HTML>
```

To checkout how this works, click here.

Both getCurrentPosition() and watchPosition() accept an optional third argument in the form of [PositionOptions](http://dev.w3.org/geo/api/spec-source.html#position_options_interface). This lets the caller control the behavior of the functions in terms of desired accuracy, if the caller is willing to accept cached location info and maximum time between the call to getCurrentPosition() (or watchPosition()) and the time at which one of the call backs are called.

It wouldn’t be entirely surprising if you have privacy concerns around sharing your physical location with a website out on the Internet. The W3C geolocation API addresses privacy concerns by mandating that a user agent must not send location information to web sites without user’s explicit consent, so that you are always in control with respect to who your location information is shared with.

While using the W3C geolocation APIs to obtain your location is quite interesting, that alone wouldn’t let you do much. Real fun starts when you club it with something like, say, Google Maps JavaScript API. That’s something we’ll probably talk about some other time!
