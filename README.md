# Sound Effect Geneartion with Pretrained AudioLDM

**Author:** Daesoo Lee  
**Company:** HANCE

## Introduction

AudioLDM is a powerful tool designed to generate high-quality sound effects from either audio input or brief text descriptions. Leveraging the latest advancements in diffusion models and contrastive learning, AudioLDM provides a versatile solution for creating similar or enhanced versions of sound effects in a fast and efficient manner. This technology is particularly useful for sound designers, game developers, and content creators who need to generate unique audio clips that closely match existing sounds or specific descriptions.

At the core of AudioLDM is the Latent Diffusion Model, which operates on compressed representations of sound. These representations are encoded using the CLAP (Contrastive Language-Audio Pretraining) model, which simultaneously processes both audio files and text descriptions. This dual encoding mechanism allows the system to capture the underlying semantics of a sound effect and its corresponding textual representation, making it capable of generating audio that is not only acoustically similar but also contextually relevant to the input.

## Method


<div style="text-align: center;">
  <img src="https://github.com/danelee2601/hacking_AudioLDM.github.io/blob/main/.img/audioldm_hacking.png?raw=true" alt="method" style="width: 100%;">
</div>

The method utilizes the pretrained AudioLDM, `audioldm_48k`, from [this Github repository](https://github.com/haoheliu/AudioLDM2). AudioLDM mainly conists of two models: a latent diffusion model (LDM) and a CLAP encoder (i.e., audio and text encoders). The main idea is to encode a query audio and/or text description via the CLAP encoders and conditionally generate a sound effect with the LDM.

1. **Input Query**: The user provides:
   - A **WAV file** containing a sound effect they wish to replicate, and/or
   - A **brief text description** of the desired sound effect.
   
   Both inputs aim to produce a sound that closely matches the original file or description.

2. **CLAP Encoders**: The system uses two encoders from the **CLAP (Contrastive Language-Audio Pretraining)** model. 
   - The **CLAP audio encoder** processes the user's input WAV file, transforming the sound into an audio vector.
   - Simultaneously, the **CLAP text encoder** converts the user's text description into a text vector.
   
   These vectors represent the semantic and acoustic characteristics of the input in a lower-dimensional space.

3. **Latent Diffusion Model**: Both the audio and text vectors are passed through a **Latent Diffusion Model**. This model generates variations of the original sound effect based on the provided input. The diffusion process iteratively refines the latent representations of the sound, ultimately producing a new audio signal.

4. **Output**: the LDM generates an audio file that closely resembles the input sound and text description. The resulting sampling rate is 48k.


**Tips**
- `ddim_steps`: the larger value leads to better quality audio. It should be at least 200 to ensure ok-quality and maximum 500.
<!-- - `n_candidate_gen_per_text`: the higher value results in better chance of producing higher quality audio. -->
- `guidance_scale`: 
- `text_reliance_rate`: 

## Experiments

The aim is to allow users to generate variations of a selected sound effect. Every sample in Soundly comes with text description, therefore the text data is free once the user selects an audio sample. Hence, the synthetic samples in this experiments are generated based on Soundly samples with the corresponding text descriptions.

## Experimental Results


**Reference Audio:** (from Soundly)

<audio controls>
  <source src="https://github.com/danelee2601/hacking_AudioLDM.github.io/raw/refs/heads/main/.audio/search_query/Voices, Baby, Newborn, Maternity Ward, Baby Crying.wav" type="audio/mpeg">
  Your browser does not support the audio element.
</audio>

**Text Description:** "Voices, Baby, Newborn, Maternity Ward, Baby Crying" (= audio description of the Soundly sample)

| Samples |
| ------- |
| <audio controls><source src="https://github.com/danelee2601/hacking_AudioLDM.github.io/raw/refs/heads/main/.audio/generated_samples/Voices, Baby, Newborn, Maternity Ward, Baby Crying/1.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| <audio controls><source src="https://github.com/danelee2601/hacking_AudioLDM.github.io/raw/refs/heads/main/.audio/generated_samples/Voices, Baby, Newborn, Maternity Ward, Baby Crying/2.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| <audio controls><source src="https://github.com/danelee2601/hacking_AudioLDM.github.io/raw/refs/heads/main/.audio/generated_samples/Voices, Baby, Newborn, Maternity Ward, Baby Crying/3.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |

***


**Reference Audio:** (from Soundly)

<audio controls>
  <source src="https://github.com/danelee2601/hacking_AudioLDM.github.io/raw/refs/heads/main/.audio/search_query/Voices, Baby, Laughing, Giggling.wav" type="audio/mpeg">
  Your browser does not support the audio element.
</audio>

**Text Description:** "Voices, Baby, Laughing, Giggling" (= audio description of the Soundly sample)

| Samples |
| ------- |
| <audio controls><source src="https://github.com/danelee2601/hacking_AudioLDM.github.io/raw/refs/heads/main/.audio/generated_samples/Voices, Baby, Laughing, Giggling/1.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| <audio controls><source src="https://github.com/danelee2601/hacking_AudioLDM.github.io/raw/refs/heads/main/.audio/generated_samples/Voices, Baby, Laughing, Giggling/2.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| <audio controls><source src="https://github.com/danelee2601/hacking_AudioLDM.github.io/raw/refs/heads/main/.audio/generated_samples/Voices, Baby, Laughing, Giggling/3.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |

***


**Reference Audio:** (from Soundly)

<audio controls>
  <source src="https://github.com/danelee2601/hacking_AudioLDM.github.io/raw/refs/heads/main/.audio/search_query/Vehicles, Car, Pass By, Audi Q5, Fast Speed, Asphalt.wav" type="audio/mpeg">
  Your browser does not support the audio element.
</audio>

**Text Description:** "Vehicles, Car, Pass By, Audi Q5, Fast Speed, Asphalt" (= audio description of the Soundly sample)

| Samples |
| ------- |
| <audio controls><source src="https://github.com/danelee2601/hacking_AudioLDM.github.io/raw/refs/heads/main/.audio/generated_samples/Vehicles, Car, Pass By, Audi Q5, Fast Speed, Asphalt/1.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| <audio controls><source src="https://github.com/danelee2601/hacking_AudioLDM.github.io/raw/refs/heads/main/.audio/generated_samples/Vehicles, Car, Pass By, Audi Q5, Fast Speed, Asphalt/2.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| <audio controls><source src="https://github.com/danelee2601/hacking_AudioLDM.github.io/raw/refs/heads/main/.audio/generated_samples/Vehicles, Car, Pass By, Audi Q5, Fast Speed, Asphalt/3.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |

***


**Reference Audio:** (from Soundly)

<audio controls>
  <source src="https://github.com/danelee2601/hacking_AudioLDM.github.io/raw/refs/heads/main/.audio/search_query/Aircraft, Helicopter.wav" type="audio/mpeg">
  Your browser does not support the audio element.
</audio>

**Text Description:** "Aircraft, Helicopter" (= audio description of the Soundly sample)

| Samples |
| ------- |
| <audio controls><source src="https://github.com/danelee2601/hacking_AudioLDM.github.io/raw/refs/heads/main/.audio/generated_samples/Aircraft, Helicopter/1.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| <audio controls><source src="https://github.com/danelee2601/hacking_AudioLDM.github.io/raw/refs/heads/main/.audio/generated_samples/Aircraft, Helicopter/2.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| <audio controls><source src="https://github.com/danelee2601/hacking_AudioLDM.github.io/raw/refs/heads/main/.audio/generated_samples/Aircraft, Helicopter/3.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |

***


**Reference Audio:** (from Soundly)

<audio controls>
  <source src="https://github.com/danelee2601/hacking_AudioLDM.github.io/raw/refs/heads/main/.audio/search_query/Alarms, Bell, Fire Alarm, Short Buzzes, Distant, Stairway.wav" type="audio/mpeg">
  Your browser does not support the audio element.
</audio>

**Text Description:** "Alarms, Bell, Fire Alarm, Short Buzzes, Distant, Stairway" (= audio description of the Soundly sample)

| Samples |
| ------- |
| <audio controls><source src="https://github.com/danelee2601/hacking_AudioLDM.github.io/raw/refs/heads/main/.audio/generated_samples/Alarms, Bell, Fire Alarm, Short Buzzes, Distant, Stairway/1.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| <audio controls><source src="https://github.com/danelee2601/hacking_AudioLDM.github.io/raw/refs/heads/main/.audio/generated_samples/Alarms, Bell, Fire Alarm, Short Buzzes, Distant, Stairway/2.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| <audio controls><source src="https://github.com/danelee2601/hacking_AudioLDM.github.io/raw/refs/heads/main/.audio/generated_samples/Alarms, Bell, Fire Alarm, Short Buzzes, Distant, Stairway/3.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |

***

**Reference Audio:** (from Soundly)

<audio controls>
  <source src="https://github.com/danelee2601/hacking_AudioLDM.github.io/raw/refs/heads/main/.audio/search_query/Cartoon, Animal, Duck, Quacks, Whistle.wav" type="audio/mpeg">
  Your browser does not support the audio element.
</audio>

**Text Description:** "Cartoon, Animal, Duck, Quacks, Whistle" (= audio description of the Soundly sample)

| Samples |
| ------- |
| <audio controls><source src="https://github.com/danelee2601/hacking_AudioLDM.github.io/raw/refs/heads/main/.audio/generated_samples/Cartoon, Animal, Duck, Quacks, Whistle/1.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| <audio controls><source src="https://github.com/danelee2601/hacking_AudioLDM.github.io/raw/refs/heads/main/.audio/generated_samples/Cartoon, Animal, Duck, Quacks, Whistle/2.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| <audio controls><source src="https://github.com/danelee2601/hacking_AudioLDM.github.io/raw/refs/heads/main/.audio/generated_samples/Cartoon, Animal, Duck, Quacks, Whistle/3.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |

***


**Observed Limitations**
- It has difficult with capturing temporal dynamics of a reference audio. That is probably due to a high compression rate by collapsing the temporal dimension to produce CLAP embeddings.
- Uncommmon types of sound effect are difficult to produce variations for. That is probably because the dataset that the pretrained AudioLDM was trained on did not include much of those types.
- Difficult to produce variations for sound effects with short length. This is because all the used training samples are 10s. This is the audio length bias problem I mentioned many times.


