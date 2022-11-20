
What is Modsia
==============

The **Modsia** project aims to provide a comprehensive architecture and platform for speech
interaction on common Linux desktop environments.

The project both defines an extensible
architecture for suporting voice input and provides a minimal reference implementation of each of
the key components of that architecure.


Freedom of Speech Interaction
-----------------------------

For most people, speech is their primary and most natural mode of interaction with other humans. In
recent years, speech has also become a common way for humans to interact with their smartphones and
IoT devices. For some, speech interaction is a great convenience; for those with visual impairment
it can open up oportunities that would otherwise be closed. Advances in speech recognition and
speech interaction have had huge impact on usability and accessibility.

Unfortunately, until now these advances have not been shared with the Linux desktop
community. While Open Source Software has been a major driver of the improvements in speech-to-text
algorithms, most of the usability and accessibility improvements have either been limited to closed
source platforms or have required use of commercial cloud services.

**Modsia** aims to redress this situation by providing a Free architecture for integrating speech
interaction into Free and Open Source desktop environments on platforms such as Linux. It and also
aims to deliver an open source reference implementation of the necessary components, both to offer
an immediate option for deploying speech interaction, and to allow others to innovate, iterate and
improve its capabilities.


Speech Interaction and Speech Input
-----------------------------------

There are two main use cases for speech recognition and speech-to-text conversion in a desktop
environment: voice commands for interaction, and text input through dictation. Both of these
involve converting audio input into computer input, but they have different requirements and impose
different demands on the system.

Speech Interaction
******************

Most modern technology users are familiar with speech interaction through voice-driven assistants
such as Siri or Alexa. With these systems users can issue commands to a computer, execute simple
tasks and perform searches by speaking short phrases. These are often prefaced by an attention word
or phrase (such as "Hey, Siri"). Generally these commands have a limited vocabulary and a limited
grammar, and the entire phrase is uttered before the system attempts to understand it.


Speech Input
************

Generalised speech dictation for text input is a much more complex task than command recognition,
and perhaps a more valuable one, especially for those who are visually impaired. During
dictation the vocabulary is typically much larger and the grammar is less constrained. Dictation is
often less clearly spoken than short commands are. Dictation is an incremental process; users
expect to see the input as they go along rather than waiting until the end. Furthermore, users
will *um* and *err*, repeat words, backtrack and change their minds as they go along.


**Modsia** aims to provide support for both command-based speech interaction and dictation for
general speech input.
