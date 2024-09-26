# Sound Effect Vector Search Demo

**Author:** Daesoo Lee  


## Introduction

Welcome to the demo of our new method for Sound Effect Vector Search. This project aims to find the top **k** most similar sound effect files from the **Soundly database** based on a given audio file and/or text description. By providing both an audio reference and a text description, users can achieve the most accurate search results. However, the system is flexible and allows searches using either input individually.

## Method


<div style="text-align: center;">
  <img src="https://github.com/danelee2601/hacking_AudioLDM.github.io/blob/main/.img/audioldm_hacking.png?raw=true" alt="method" style="width: 100%;">
</div>

The method utilizes the pretrained AudioLDM, `audioldm_48k`, from [this Github repository](https://github.com/haoheliu/AudioLDM2). AudioLDM mainly conists of two models: a latent diffusion model (LDM) and a CLAP encoder (i.e., audio and text encoders). The main idea is to encode a query audio and text description via the CLAP encoders and conditionally generate a sound effect with the LDM.

1. **Input Query**: The user provides:
   - A **WAV file** containing a sound effect they wish to replicate, and
   - A **brief text description** of the desired sound effect.
   
   Both inputs aim to produce a sound that closely matches the original file or description.

2. **CLAP Encoders**: The system uses two encoders from the **CLAP (Contrastive Language-Audio Pretraining)** model. 
   - The **CLAP audio encoder** processes the user's input WAV file, transforming the sound into an audio vector.
   - Simultaneously, the **CLAP text encoder** converts the user's text description into a text vector.
   
   These vectors represent the semantic and acoustic characteristics of the input in a lower-dimensional space.

3. **Latent Diffusion Model**: Both the audio and text vectors are passed through a **Latent Diffusion Model**. This model generates variations of the original sound effect based on the provided input. The diffusion process iteratively refines the latent representations of the sound, ultimately producing a new audio signal.

4. **Output**: the LDM generates an audio file that closely resembles the input sound and text description. 


**Tips**
- `ddim_steps`: the larger value leads to better quality audio. It should be at least 200 to ensure ok-quality and maximum 500.
- `n_candidate_gen_per_text`: the higher value results in better chance of producing higher quality audio.
- `guidance_scale`: 
- `text_reliance_rate`: 

## Experiments

## Experimental Results
Below we present sample results from our system. Audio samples are provided to demonstrate the effectiveness of our search method.

### A. User Input: Reference Audio + Text Description

**Reference Audio:** (from the BBC sound effect library)

<audio controls>
  <source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/search_query/bicycle_bell.wav" type="audio/mpeg">
  Your browser does not support the audio element.
</audio>

**Text Description:** "The sound of a bicycle bell."

| Samples |
| ------- |
| <audio controls><source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/soundly_samples/SND33601.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| <audio controls><source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/soundly_samples/SND33601.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |


<!-- | Rank | Similar Audio (from Soundly) |
|------|---------------|
| 1    | <audio controls><source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/soundly_samples/SND33601.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| 2    | <audio controls><source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/soundly_samples/SND6604.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| 3    | <audio controls><source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/soundly_samples/SND6603.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| 4    | <audio controls><source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/soundly_samples/SND49674.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| 5    | <audio controls><source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/soundly_samples/SND7698.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> | -->


### B. [Ablation Study] User Input: Reference Audio Only

| Rank | Similar Audio (from Soundly) |
|------|---------------|
| 1    | <audio controls><source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/soundly_samples/SND51961.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| 2    | <audio controls><source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/soundly_samples/SND51962.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| 3    | <audio controls><source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/soundly_samples/SND51960.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| 4    | <audio controls><source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/soundly_samples/SND59826.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| 5    | <audio controls><source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/soundly_samples/SND59824.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |


### C. [Ablation Study] User Input: Text Description Only

| Rank | Similar Audio (from Soundly) |
|------|---------------|
| 1    | <audio controls><source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/soundly_samples/SND60566.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| 2    | <audio controls><source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/soundly_samples/SND33601.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| 3    | <audio controls><source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/soundly_samples/SND60565.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| 4    | <audio controls><source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/soundly_samples/SND33602.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| 5    | <audio controls><source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/soundly_samples/SND60571.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |






## Additional Experimental Results

**Reference Audio:** (from the Game of Thrones, episode "The Bells"; you can hear the dragon at 11s!)

<audio controls>
  <source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/search_query/bell_game_of_throne.wav" type="audio/mpeg">
  Your browser does not support the audio element.
</audio>

**Text Description:** "The sound of a church bell."

NB! Even if there's some noise (e.g., dragon roaring) included in a reference audio sample, a text description will guide the search so that users get what they want.

| Rank | Similar Audio (from Soundly) |
|------|---------------|
| 1    | <audio controls><source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/soundly_samples/SND63543.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| 2    | <audio controls><source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/soundly_samples/SND80488.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| 3    | <audio controls><source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/soundly_samples/SND91714.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| 4    | <audio controls><source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/soundly_samples/SND37051.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| 5    | <audio controls><source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/soundly_samples/SND93410.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
