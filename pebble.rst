Pebble
======
The Pebble connects to an android device over bluetooth, and requires the Pebble Application on the phone. The app can be downloaded from the play store.

To use Pebble with Wearscript, the Wearscript pebble client must be installed on the Pebble. Installation instructions are included in the repository.

This script demonstrates pebble click events, setting text in the view, vibration, and using accelerometer data.

.. code-block:: html
    
    <html style="width:100%; height:100%; overflow:hidden">
    <body style="width:100%; height:100%; overflow:hidden; margin:0">
    <script>
    function main() {
        if (WS.scriptVersion(1)) return;
        WS.pebbleSetBody('connected!', false);
        
        WS.sensorOn(WS.sensor("pebbleAccelerometer"), .25, function (data) {
            WS.log('x: ' + data['values'][0] + ' y: ' + data['values'][1] + ' z: ' + data['values'][2]);
            WS.pebbleSetBody('x: ' + data['values'][0] + ' y: '+ data['values'][1] + ' z: ' + data['values'][2], false);
        });
        
        WS.pebbleSetSubtitle('Accel Data', false);
        
        WS.gestureCallback('onPebbleSingleClick', function (name) {
            WS.log('onPebbleSingleClick: ' + name);
            WS.pebbleSetTitle(name + ' clicked', false);
            WS.say(name);
        });
        
        WS.gestureCallback('onPebbleLongClick', function (name) {
            WS.log('onPebbleLongClick: ' + name);
            WS.pebbleVibe(2);
        });
        
        WS.gestureCallback('onPebbleAccelTap', function (axis, direction) {
            WS.log('onPebbleAccelTap: ' + axis + ' ' + direction);
        });
    }
    window.onload = main;
    </script>
    </body>
    </html>   


You can also subscribe to pebble gestures.

.. code-block:: html

  <html style="width:100%; height:100%; overflow:hidden">
  <body style="width:100%; height:100%; overflow:hidden; margin:0">
  <script>
  function main() {
      if (WS.scriptVersion(1)) return;
      WS.subscribe("gesture:pebble:singleClick:UP", function () {
          WS.control('SWIPE_LEFT');
      });
      WS.subscribe("gesture:pebble:singleClick:DOWN", function () {
          WS.control('SWIPE_RIGHT');
      });
      WS.subscribe("gesture:pebble:singleClick:SELECT", function () {
          WS.control('TAP');
      });
      WS.subscribe("gesture:pebble:longClick:SELECT", function () {
          WS.control('SWIPE_DOWN');
      });
  }
  window.onload = main;
  </script></body></html>

API
===

pebbleSetTitle(String text, boolean clear) : void
  Set the title text area on the pebble view. 

pebbleSetSubtitle(String text, boolean clear) : void
  Set the subtitle text area on pebble view.

pebbleSetBody(String text, boolean clear) : void
  Set the body text area on pebble view.
  
  :text(string): text to display on pebble
  :clear(boolean): boolean to clear other text areas (true clears all other text areas) 

pebbleVibe(int vibeType)
  Vibrate pebble.

======== =======
  Type    Value
======== =======
short     0
long      1
double    2
======== =======

