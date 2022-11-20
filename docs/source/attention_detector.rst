The Attention Detector
======================

This is a stub page.

Simple API into a shared library. Library selection either from an explicit path configuration or from
a pre-defined path. Most users will switch using alternatives mechanism.

The API includes listing known wake words. This might be a single built-in word, a short list of known
words or a small set of word pattern files, depending on the library. The API also allows for the
hub to query the required sample rate and sample format.

A single detection session can be started with a specified wake word. The word can be changed by
closing the session and starting a new one.

During operation, blocks of audio samples are passed in and a boolean result is returned to indicate
if the wake word was spoken.
