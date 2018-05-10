---
templateKey: blog-post
title: Simple JavaScript Clock with Time Zone Support
date: '2013-07-29T12:10:00-07:00'
tags:
  - clock
  - javascript
  - time zone
---
Just wanted to write down a quick little JavaScript clock I made that converts to the user's time zone.  Since we have offices all over the world and the date time fields keep track of time in UTC it is important for users to know what time zone they are currently logged into.  I have placed this clock in the navbar at the top of the page next to the user to let them know what the current time is for their settings.  If they need to change it they can do so by clicking on it and changing their user profile.  Here is the code and feel free to <a href="https://github.com/ericsaupe/worldclock-js" target="_blank">fork this project at GitHub</a>.

<pre><code>$(document).ready(function() {
   //Get user office for UTC offset
   var office = $("[name='office']").val();
   startTime(office);
});

function startTime(office){
    //Extra functionality for Date to calculate DST
    Date.prototype.stdTimezoneOffset = function() {
        var jan = new Date(this.getFullYear(), 0, 1);
        var jul = new Date(this.getFullYear(), 6, 1);
        return Math.max(jan.getTimezoneOffset(), jul.getTimezoneOffset());
    }
    Date.prototype.dst = function() {
        return this.getTimezoneOffset() &lt; this.stdTimezoneOffset();
    }

    var offset = 0;
    if (office == 0 || office == 1) {
        //SLC
        offset = -7;
    }
    else if (office == 2) {
        //Switzerland
        offset = 1;
    }
    else if (office == 3) {
        //China
        offset = 8;
    }
    else if (office == 4) {
        //Taiwan
        offset = 8;
    }
    var now = new Date();
    //Calculate Daylight Savings time in areas where that matters
    if (office &lt; 3) {
        if (now.dst()) { offset += 1; }   
    }
    var today = new Date(now.getUTCFullYear(), now.getUTCMonth(), now.getUTCDate(),  now.getUTCHours() + offset, now.getUTCMinutes(), now.getUTCSeconds());
    var h = today.getHours();
    var m = today.getMinutes();
    // add a zero in front of numbers &lt; 10
    m = checkTime(m);
    ampm = h &gt;= 12 ? "pm" : "am"
    //12-Hour Clock
    h = h % 12
    if (h == 0) {
      h = 12;
    }
    document.getElementById('clock').innerHTML=h+":"+m+" "+ampm;
    t=setTimeout(function(){startTime(office)},500);
}

function checkTime(i){
    if (i &lt; 10) {
        i = "0" + i;
    }
    return i;
}
</code></pre>

Let's break it down a bit.  At the beginning we get the user's current office which is held in an input hidden in a modal on every page.  We then run the startTime function and begin our journey.  First we add a couple of features to the JavaScript Date to help us account for day light savings time in countries that follow that.  Then we figure out the time zone offset from UTC time depending on the office.  This offset is not including day light savings time which is what gets calculated next.  We get the current date by calling new Date().  This gets the system time which we use to see if it is day light savings time.  Day light savings time is not followed in China and so the check for office number is there to not do the calculation for China and Taiwan.  We then convert the current time to UTC and add the offset to the hours.

At this point we just need to output our result to be displayed.  The checkTime function is called to add a zero in front of numbers less than 10.  Then we get AM/PM and calculate for a 12 hour clock.  Modding by 12 will give us the current hour for all cases except noon and midnight where 0 is returned.  In these cases we simply check and return 12.  Then we output to our div and we're done.  Then we run a timer to run every half-second.  It may run a bit fast but the results are negligible when it has a run-time of a certain amount of milliseconds and the timer gets recalculated at each page refresh it is not noticeable.  I'd rather have it a moment or two fast than slow.

I got a lot of this code from Googleing around but altered it almost entirely to fit my needs.  I couldn't find anything that consistently accounted for time zones and day light savings time easily so I wrote my own.  Now when users log into our site and are looking at data they can make sure they are seeing the times for their office and won't need to calculate time zone conversions on their own.

Final product!  Works great.  Everyone I've showed seems to love it and I'm pretty proud of it.

![null](/img/screenshot-from-2013-07-29-171006.png)

EDIT:

Here's a shot when the user clicks on the time.  It displays the user's office.  Clicking on the office will bring up their profile where they can change this setting.

![null](/img/screenshot-from-2013-07-29-171302-300x75.png)
