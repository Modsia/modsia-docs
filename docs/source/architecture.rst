High Level Architecture
=======================

The *"M"* in in Modsia stands for *modular*. Any implementation of the architecture will be made up of
a number of modules which in general will function independently of each other. The goal is that any
one of them could be replaced and the other modules would neither know nor care.

Modularity is important to our goals for two main reasons. Firstly, the field of Speech-to-Text
conversion is a fast evolving one. Rapid advances are being made in both the algorithms and the data
sets on which they are trained, so what is considered state-of-the-art today may well be considered a
poor performer very soon and it should be easy to slot in a better implementation. Secondly, machines
in the the Linux desktop world span a very wide variety of hardware capabilities. Linux laptops range
from the light weight and low power to luggable desktop replacements while Linux desktop hardware
ranges from Raspberry Pis to decked-out gaming PCs. Users need to be able to make the tradeoffs
between performance and resource usage that best fit their needs.

Component Interaction
---------------------

Since Modsia is made up of a number of modules which run independently, these modules need to be
able to find each other and communicate with each other. Fortunately, in the Linux desktop world
there is already a mechanism for solving this sort of problem, `D-Bus <http://dbus.freedesktop.org/>`_,
the Desktop Bus. D-Bus supports most of the the necessary discovery and inter-process communication
needs of Modsia and in general most modules will interact with each other by calling methods on
objects over the desktop *session* D-Bus.

Although D-Bus is the way that most of the components interact with each other, there is one exception:
the Attention Detector. This module, is typically small, has low resource needs, serves a very narrow
purpose and benefits from very low latency. As a result the Attention Detector is delivered as a
dynamically loaded shared library.

The diagram below is a rough sketch of the flow of data between the main components of the system.

.. graphviz:: components.dot


Session Hub
-----------

The core of Modsia is the Session Hub.  It is responsible for coordinating the actions of all of the
other modules Typically it will be started when the user logs in to a desktop session and will run
until they log out. During this time it loops, performing a set of core functions which mostly involve
collecting data from and/or passing data to other modules. These functions include:

* Maintaining an open channel to the audio source. If the channel closes it will reopen the channel.
  If a difference source is configured then it will close the current channel and open a new channel.

* Performing *squelch* and activity detection. The audio source in constantly monitored but the input
  is ignored unless the signal rises above a threshold of background noise.

* Reading audio data and feeding it to the attention detector. When the audio signal rises above the
  background threshold the attention detector will attempt to identify the "wake word" that
  signals that the user wants to start interacting.

* Streaming data to STT engine. Once the user has Modsia's attention, the Session Hub will feed
  subsequent audio data to one of the configured speech-to-text engines for conversion. Which engine
  will depend on the context, as discussed below.

* Passing command text to Skills Interpreter. When handling commands, the Skill interpreter will try
  to match the spoken command to one of the "skill" patterns configured by the user. If a match is
  found, the corresponding action script or action is executed by the Skills Interpreter.

* Send text stream as series of paste and edit events for speech input. When handling dictation, the
  STT engine will return incremental text output, possibly including back-tracking edits to earlier
  text. The Session Hub will feed these through the D-Bus session to the currently active application.



Attention Detector
------------------

The Attention Detector is designed to be a small, light-weight and low latency library that can
rapidly check if the user has spoken the "wake word" attention phrase. It is important that this
code uses as little CPU as possible since it will need to process all audio input the the machine
whenever that input rises above the noise threshold.

In order to minimise the overhead in calling the Attention Detector, it is the only module that is
delivered as a dynamically loaded shared library rather than running in its own process and being
called over an IPC mechanism. The API for the module is very simple; aside from module initialisation
and enumeration of the known wake words, the main call takes a block of audio and returns a boolean
indicating if the recent audio includes the attention word. Calls are synchronous and should be low
latency.

It is expected that the Attention Detector shared library will be accessible at a know file path
which may be a soft link in order to support *alternatives*. A detector library may support a single
word, natively support multiple words or load one from a set of word template files.

Speech-to-Text Engine
---------------------

The Speech-to-Text (STT) engine handles the core function of converting audio data into words. This
is a complex task and there are many different approaches to solving it, each with their benefits and
drawbacks, and the science underpinning these is evolving rapidly. As a result, Modsia allows the user
to make use of more than one STT engine in a single system, with different engines being used in
different situations.

There are many criteria for choosing an STT engine: Is the engine capable of handing continuous
audio or does it only work on distinct segments? What are the CPU and memory requirements of the
engine? What are the word error rates? Does it require use of a GPU? Does the engine run locally or
is the work performed by a remote service that needs to be on-line? As a result it may make sense to
switch engines depending on where the user is and what else the user is doing.

The API for the STT engine provides access to metadata about the engine as a whole and about any
different speech and language models that it might offer. A user can then configure the Session Hub
with an ordered list of engines and models to use when conditions allow.

It should be noted that most modern STT engines do not typically return a single interpretation of
for a given audio input. Instead they return a set of results, each tagged with a score or probability.
In the case of continuous transcription it is also often the case that the STT engine may "change its
mind" regarding the probability of earlier words based on later words. The Session Hub must handle
these sorts of results appropriately.

The API for the STT engine allows both for the provision of lists of proper nouns and other
domain-specific words and for feedback when the user or the Skills Interpreter indicates that a
different interpretation of the input is more accurate than the first choice of the STT engine.


Skills Interpreter
------------------

When Modsia is handling commands, as opposed to taking dictation, the Skills Interpreter takes the
text interpretation (or scored multiple interpretations) from the STT engine and tries to work out
what command is being requested by the user. As with the STT, there are potentially many approaches
to solving this problem, ranging from simple pattern matching to complex Natural Language
Understanding, but in essence the Skills Interpreter will maintain a list of templates for the
commands that it can understand, and for any given input, it attempts to find the best match from
the known commands.

Each command template has an associated script that is executed when a match is found. This might be
a simple executable command or it can involve the substitution of extracted parts of the input text.
The Skills Interpreter also supports directly making calls on the D-Bus, since this is one of the
primary ways in which desktop actions are triggered.

It should be noted that since the Skills Interpreter takes text input, it is possible to use it to
provide a generic, text-based, natural language interface to the desktop environment.



Each of these components is discussed in the following chapters.

