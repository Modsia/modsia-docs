The Session Hub
===============

This is a stub page.


Configuration
-------------

The configuration settings are introspectable, so that new settings can be handled without needing
to build a complete new UI.

Where possible, configuration changes are used immediately.

Audio sources
-------------

Use PulseAudio for now, because it's still the standard.

Sensibly handle default source selection.

Handle D-Bus notifications when some other application needs to grab exclusive use of the audio input,
for instance when a call is being made on a videoconferencing application.


STT engine selection
--------------------

The hub tracks if the system has a current connection to the internet and will not use an on-line
STT engine if not on line.

Possibly support avoiding use of the GPU when the GPU memory is full.


Performance
-----------

Shunting blocks of audio data should ideally be low latency and low overhead.


