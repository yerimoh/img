The general purpose of our model is to achieve
multitask learning by a mapping function (f) between input (x), output (y), and schema (s), i.e.,
f(x, s) = y. Due to the multitasking nature of
our setting, both inputs and outputs can originate
from different tasks n, i.e. x = [xt1, xt2, ...xtn]
and y = [yt1, yt2, ...ytn], all of which fit under a common schema (s). Given the presence
of domain-specific materials science language,
our model architecture includes a domain-specific
BERT encoder and a transformer decoder. All
BERT encoders and transformer decoders share
the same general architecture, which relies on a
self-attention mechanism: Given an input sequence
of length N, we compute a set of attention scores,
A = softmax(QTK/(
√
dk)). Next, we compute
the weighted sum of the value vectors, O = AV ,
where Q, K, and V are the query, key, and value
matrices, and dk is the dimensionality of the key
vectors.


Additionally, the transformer based decoder differ from the domain specific encoder by: 1) Applying masking based on the schema applied to
ensure that it does not attend to future positions
in the output sequence. 2) Applying both selfattention and encoder-decoder attention to compute attention scores that weigh the importance
of different parts of the output sequence and input sequence. The output of the self-attention
mechanism (O1) and the output of the encoderdecoder attention mechanism (O2) are concatenated and linearly transformed to obtain a new
hidden state, H = tanh(Wo[O1; O2] + bo) with
Wo and bo being the weight and biases respectively. The model then applies a softmax to H
to generate the next element in the output sequence
P = softmax(WpH + bp) , where P is a probability distribution over the output vocabulary.
