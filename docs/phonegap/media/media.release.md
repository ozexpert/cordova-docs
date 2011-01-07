media.release
=================

Releases the underlying operating systems audio resources.

    media.release();


Description
-----------

Function `media.release` is a synchronous function that releases the underlying operating systems audio resources.  This function is particularily important for Android as there are a finite amount of OpenCore instances for media playback.  Developers should call the 'release' function when they no longer need the Media resource.

Supported Platforms
-------------------

- Android
    
Quick Example
-------------

        // Audio player
        //
        var media = new Media(src, onSuccess, onError);
        
        media.play();
        media.stop();
        media.release();

Full Example
------------

    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN"
                          "http://www.w3.org/TR/html4/strict.dtd">
    <html>
      <head>
        <title>Device Properties Example</title>

        <script type="text/javascript" charset="utf-8" src="phonegap.js"></script>
        <script type="text/javascript" charset="utf-8">

        // Wait for PhoneGap to load
        //
        function onLoad() {
            document.addEventListener("deviceready", onDeviceReady, false);
        }

        // PhoneGap is ready
        //
        function onDeviceReady() {
            playAudio("http://audio.ibeat.org/content/p1rj1s/p1rj1s_-_rockGuitar.mp3");
        }
    
        // Audio player
        //
        var media = null;
        var mediaTimer = null;

        // Play audio
        //
        function playAudio(src) {
            // Create Media object from src
            var media = new Media(src, onSuccess, onError);

            // Play audio
            media.play();

            // Update media position every second
            if (mediaTimer == null) {
                mediaTimer = setInterval(function() {
                    // get media position
                    media.getCurrentPosition(
                        // success callback
                        function(position) {
                            if (position > -1) {
                                setAudioPosition((position/1000) + " sec");
                            }
                        },
                        // error callback
                        function(e) {
                            console.log("Error getting pos=" + e);
                            setAudioPosition("Error: " + e);
                        }
                    );
                }, 1000);
            }
        
            // Get duration
            var counter = 0;
            var timerDur = setInterval(function() {
                counter = counter + 100;
                if (counter > 2000) {
                    clearInterval(timerDur);
                }
                var dur = media.getDuration();
                if (dur > 0) {
                    clearInterval(timerDur);
                    document.getElementById('audio_duration').innerHTML = (dur/1000) + " sec";
                }
           }, 100);
        }

        // Pause audio
        // 
        function pauseAudio() {
            if (media) {
                media.pause();
            }
        }

        // Stop audio
        // 
        function stopAudio() {
            if (media) {
                media.stop();
            }
            clearInterval(mediaTimer);
            mediaTimer = null;
        }

        // Release audio
        // 
        function releaseAudio() {
            if (media) {
                media.release();
            }
        }

        // onSuccess Callback
        //
        function onSuccess() {
            console.log("playAudio():Audio Success");
        }
    
        // onError Callback 
        //
        function onError(error) {
            alert('code: '    + error.code    + '\n' + 
                  'message: ' + error.message + '\n');
        }

        // Set audio position
        // 
        function setAudioPosition(position) {
            document.getElementById('audio_position').innerHTML = position;
        }

        </script>
      </head>
      <body onload="onLoad()">
        <a href="#" class="btn large" onclick="playAudio();">Play Audio</a>
        <a href="#" class="btn large" onclick="pauseAudio();">Pause Playing Audio</a>
        <a href="#" class="btn large" onclick="stopAudio();">Stop Playing Audio</a>
        <a href="#" class="btn large" onclick="releaseAudio();">Release Audio</a>
        <p id="audio_position"></p>
        <p id="audio_duration"></p>
      </body>
    </html>
