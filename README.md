# Sound Effect Vector Search Demo

**Author:** Daesoo Lee  


## Introduction

Welcome to the demo of our new method for Sound Effect Vector Search. This project aims to find the top **k** most similar sound effect files from the **Soundly database** based on a given audio file and/or text description. By providing both an audio reference and a text description, users can achieve the most accurate search results. However, the system is flexible and allows searches using either input individually.

## Method


<div style="text-align: center;">
  <img src="https://github.com/danelee2601/audio_vector_search.github.io/blob/main/.img/vector_search_method.png?raw=true" alt="method" style="width: 100%;">
</div>

The image illustrates a method for retrieving the most similar sound effects from a database based on a given input sound or text description, using vector search. Here's how the process works:

1. **Soundly Database**: This is the primary source of sound effects stored in WAV file format. These files are typically large (several MBs each).
   
2. **Vectorization of Sound Files**: All the sound files in the Soundly database are vectorized using a **CLAP (Contrastive Language-Audio Pretraining)** encoder. The CLAP encoder transforms the sound files into vectors (embeddings), which represent the audio in a lower-dimensional space. These vectors are much smaller (4KB per vector).

3. **Soundly Vector Database**: Once vectorized, the sound files are stored in a vector database, which is more compact and efficient for similarity searches.

4. **Input Query**: The user can input either or both:
   - A **WAV file** of a sound effect, which will be processed by the **CLAP audio encoder** to generate a vector, 
   - A **text description** of the desired sound effect, which is vectorized using the **CLAP text encoder**.

5. **Similarity Search**: The system compares the input vector (from either the audio and/or text query) to the vectors stored in the Soundly vector database. Using similarity metrics, the system retrieves the **k most similar sound effect samples** to the input.
    <details close>
    <summary>Mathematical details of the similarity score</summary>
    The similarity score is computed using a (joint) probability. For instance, given the query audio and text embedding vectors, denoted by $a_q$ and $t_q$, we measure correlation coefficients between the query vectors and the vectors in the database, eventually obtaining the coefficients for all vectors in the database. Those coefficients are clipped between 0 and 1 and used as probabilities. The probabilities are referred to as $p(a_i | a_q)$ and $p(a_i | t_q)$ where $a_i$ is the $i$-th vector in the vector database.

    The joint porbability, $p(a_i | a_q, t_q)$, is what we want to obtain. This can be reexpressed as follows: <br>

    $= p(a_q, t_q | a_i) p(a_i)$ <br>
    $= p(a_q | a_i) p(t_q | a_i) p(a_i)$  &nbsp; # with the independence assumption <br>
    $= p(a_q | a_i) p(t_q | a_i)$  &nbsp; # $p(a_i)$ is a constant, therefore removed <br>
    $= p(a_i | a_q) p(a_i | t_q)$  &nbsp; # $p(a_q | a_i) = p(a_i = a_q)$ because $corr(a_i,a_q) = corr(a_q,a_i)$ where $corr$ denotes a correlation function. <br>

    Therefore, we compute the similarity score by $p(a_i | a_q) p(a_i | t_q)$.
    </details>

6. **Output**: The user receives the most similar sound effects from the database based on their input (either from an audio file or a brief text description).


<!-- ## Computational Efficiency

Efficiency is a critical aspect of our system, especially when dealing with large databases like Soundly. To ensure rapid search responses, we have implemented:

- **Vector Indexing:** Using efficient data structures like KD-Trees for quick nearest neighbor searches.
- **Dimensionality Reduction:** Applying techniques such as PCA to reduce computation without significant loss of accuracy.
- **Parallel Processing:** Leveraging multi-threading and GPU acceleration where applicable. -->

## Experiments

### Experimental Setup
For the sake of initial experiments, the sound files under the "Bells" category in the Soundly database (635 samples) are used for the vector database.

## Experimental Results
Below we present sample results from our system. Audio samples are provided to demonstrate the effectiveness of our search method.

### A. User Input: Reference Audio + Text Description

**Reference Audio:** (from the BBC sound effect library)

<audio controls>
  <source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/search_query/bicycle_bell.wav" type="audio/mpeg">
  Your browser does not support the audio element.
</audio>

**Text Description:** "The sound of a bicycle bell."

| Rank | Similar Audio (from Soundly) |
|------|---------------|
| 1    | <audio controls><source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/soundly_samples/SND33601.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| 2    | <audio controls><source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/soundly_samples/SND6604.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| 3    | <audio controls><source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/soundly_samples/SND6603.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| 4    | <audio controls><source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/soundly_samples/SND49674.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
| 5    | <audio controls><source src="https://github.com/danelee2601/audio_vector_search.github.io/raw/refs/heads/main/.audio/soundly_samples/SND7698.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |


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
