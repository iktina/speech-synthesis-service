# Neural Speech Synthesis


## Welcome

The service receives a text file and uses it as input to the composition of pre-trained neural models.

## What’s the point?

The service makes text to speech transfer using machine learning techniques.
The service outputs a binary audio data containing text recorded in the specified text file voiced by the synthesized voice.

## Model details:

The service receives a raw English text sentence and uses it as input to the advanced Tacotron2-based model, followed by the WaveGlow vocoder, both of which were consecutively trained on the open LJ Speech dataset and outputs the speech audio samples as a binary file. Both models run on the same P100 GPU to ensure the fastest data flow between models and duplicated to provide more service bandwidth. The scaling factor is 2xServices per 1xP100 GPU. The input sequence is limited to140 characters, longer sentences should be splitted.

## How does it work?

The user must provide the following inputs in order to start the service and get a response:

Inputs:

 -   `endpoint`: nss.naint.tech.
 -   `method`: t2s.
 -   `input_path`: Path to '\*.txt' file containing JSON representation of input argument 'text' and its value - text for speech synthesis.
 -   `output_path`: Path to '\*.wav' output audio file.

Example of input file content:

```
{"text": "After a time she heard a little pattering of feet in the distance, and she hastily dried her eyes to see what was coming."}
```

You can call the service from SingularityNET CLI (`snet`).

Assuming that you have an open channel (`id: 0`) to this service:

```
$ snet client call --save-field data samples/sample.wav 0 0.1 nss.naint.tech t2s samples/sample.txt

Read call params from the file: samples/sample.txt
```

## What to expect from this service?

Input text:
"After a time she heard a little pattering of feet in the distance, and she hastily dried her eyes to see what was coming."

Response:
samples/sample.wav
