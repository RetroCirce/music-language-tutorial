# MusicGEN

In this section, we take [MusicGEN](https://musicgen.com/) {cite}`musicgenerationtemplate`.  as an example to introduce the auto-regressive modeling of the music generative model via the transformer architecture {cite}`musicgenerationtemplate`. 

## Neural Audio Codec

MusicGEN is an auto-regressive text-to-music generative model. The input of the MusicGEN leverages the Encodec {cite}`musicgenerationtemplate` to process music time-domain signals into discrete tokens as neural audio codec tokens. 

<center><img alt='generation_encodec' src='../_images/generation/encodec.PNG' width='50%' ></center>

As illustrated in the figure above, the Encodec architecture consists of 1D convolutional and 1D deconvolutional blocks in its encoder and decoder networks. The bottleneck block features a multi-step residual vector quantization (RVQ) mechanism, which converts the continuous latent music embeddings from the encoder into discrete audio tokens. The objective of the decoder is to reconstruct the input time-domain signals from these audio tokens. This reconstruction is trained using a combination of different objectives, including L1 loss in the time-domain signals, L1 loss in the mel-spectrogram signals, L2 loss in the mel-spectrogram signals, and adversarial training with multi-resolution STFT discriminators.

The pretrained Encodec model preprocesses the music time-domain signals into audio tokens, which serve as one part of the input for the MusicGEN model.

## MusicGEN 

MusicGEN utilizes a transformer decoder architecture to predict the next audio token based on the preceding audio tokens, as illustrated by the following probability function:

<center><img alt='generation_musicgen_p1' src='../_images/generation/musicgen_p1.PNG' width='50%' ></center>
The cross-entropy loss is the training objective:
<center><img alt='generation_musicgen_l1' src='../_images/generation/musicgen_l1.PNG' width='50%' ></center>

<center><img alt='generation_musicgen_arch' src='../_images/generation/musicgen_arch.PNG' width='50%' ></center>

When incorporating text into the music generation task, MusicGEN employs two methods to condition the text for the music generation target, as illustrated in the figure above:

1. Time-domain Concatenation: utilizing the text tokens generated by the T5 model as prefix tokens preceding the audio tokens, serving as a conditioning mechanism.

2. Cross Attention: forwarding the Keys and Values (K,V) of text tokens, and the Queries (Q) of audio tokens into the cross-attention module of the MusicGEN.

The new probability function is demonstrated as:
<center><img alt='generation_musicgen_p2' src='../_images/generation/musicgen_p2.PNG' width='50%' ></center>




