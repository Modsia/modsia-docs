The Speech to Text Engine
=========================

This is a stub page.


Reading engine and model metadata
---------------------------------

Since multiple engines will be supported, each engine must describe itself in both human-readable and
machine-readable formats.

Key resource requirements must be described, including expected memory footprint (MB), expected CPU
load (MIPS), if a particular type of GPU is needed or can be used, and if a network connection is
required.

Other required metadata include language details, support for continuous conversion and benchmark
error rates.


Context-sensitive language models
---------------------------------

The API to the STT engine allows the Hub to provide lists of context or domain specific words and
phrases in advance and then indicate to the engine during processing if any of these contexts are
relevant. This can be used to pass proper names from an address book, application names, artist,
album or song names from a music library or domain-specific vocabulary that might otherwise be
unknown by or improbable in the language model. The STT is free to ignore these lists if it doesn't
know how to handle them.


Continuous vs. discreet STT conversion
--------------------------------------

All STT engines must be able to perform speech-to-text conversion of discreet segments of audio.
Optionally, engines may also support continuous conversion, in which audio data is fed to the engine
incrementally and the resulting text is updated as new audio is interpreted. This will likely result
in some earlier text being updated.

