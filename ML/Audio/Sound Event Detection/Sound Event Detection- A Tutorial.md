# Sound Event Detection: A Tutorial


## I. SOUND EVENTS IN OUR EVERYDAY ENVIRONMENT

- ***cocktail party effect***
  - Human auditory perception is highly specialized in segregating the sound sources and directing attention to the sound source of interest.
- The goal of automatic sound event detection (SED) methods is to:
  - ***what*** is happening in an audio signal
  - ***when*** it is happening
- a general-purpose sound event detection system the target sounds are **environmental sounds**
  - bird singing, car mortor
- In the literature, these are sometimes referred to as **non-speech** and **non-music** sounds, 
  - to differentiate the field of environmental sound analysis from more established speech or music analysis tasks
- everyday listening is directed towards ***identification of the sound sources***

## II. CHALLENGES IN SOUND EVENT DETECTION

- Sound events have very diverse acoustic characteristics.
- Natural environments are polyphonic, meaning that multiple sounds may be active at the same time.
- The number of possible sound event classes is unlimited
- The case is further complicated by the lack of an established
ontology for universally describing sound classes.

## III. THE GENERAL MACHINE LEARNING APPROACH FOR SOUND EVENT DETECTION

- The dominant approach to tackle the sound event detection task is based on supervised learning
- A general classification system illustrating supervised learning for sound event detection is presented in Fig. 2 【记得补下图】
-  The annotations are represented as a binary matrix in which each element represents a class being active (1) or not active (0) within short temporal segments.
-  a single sound event detection system is used to predict activities of multiple sound classes which can be active simultaneously, leading to ***multi-class multi-label classification*** in each segment.
-  Sound event detection aims to estimate the temporal activity of sounds in an audio recording
- ***Context information*** from consecutive analysis frames brings more information to learning contiguous segments corresponding to sound event instances.
-  Also the system output in consecutive segments may be ***postprocessed***
   - some are too short to be a plausible event instance, so they would be discarded.
   -  short gaps in event activity may be “filled in” under the assumption that multiple fragments form a single event instance
- Early approaches for sound event detection borrowed techniques from ***speech recognition*** and ***music information retrieval***, and were therefore based on **traditional pattern classification techniques** like 
  - Gaussian mixture models (GMMs)
  - and hidden Markov models (HMMs).
  - Sound events in general do not consist of **similar elementary units** as speech, making GMMs and HMMs less relevant for SED.
- In contrast, modern pattern classification tools, especially deep neural networks (DNNs), can perform multilabel classification more easily