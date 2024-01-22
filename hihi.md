As shown in Figure 1, a given piece of text can include multiple labels across different tasks. Given
this multitask nature of the MatSci-NLP benchmark, we propose a new and unified Task-Schema
multitask modeling method illustrated in Figure 2
that covers all the tasks in the MatSci-NLP dataset.
Our approach centers on a unified text-to-schema
modeling approach that can predict multiple tasks
simultaneously through a unified format. The
underlying language model architecture is made
up of modular components, including a domainspecific encoder model (e.g. MatBERT, MatSciBERT, SciBERT), and a generic transformer-based
decoder, each of which can be easily exchanged
with different pretrained domain-specific NLP models. We fine-tune these pretrained language models
and the decoder with collected tasks in MatSciNLP using the procedure described in Section 4.3.


The unified text-to-schema provides a more
structured format to training and evaluating lan
guage model outputs compared to seq2seq and textto-text approaches (Raffel et al., 2020; Luong et al.,
2015). This is particularly helpful for the tasks in
MatSci-NLP given that many tasks can be reformulated as classification problems. NER and Slot
Filling, for example, are classifications at the tokenlevel, while event arguments extraction entails the
classification of roles of certain arguments. Without a predefined schema, the model relies entirely
on unstructured natural language to provide the
answer in a seq2seq manner, which significantly
increases the complexity of the task and also makes
it harder to evaluate performance. The structure
imposed by text-to-schema method also simplifies
complex tasks, such as event extraction, by enabling the language model to leverage the structure
of the schema to predict the correct answer. We
utilize the structure of the schema in decoding and
evaluating the output of the language models, as
described further in Section 4.3 in greater detail.

Moreover, our unified text-to-schema approach
alleviates error propagation commonly found in
multitask scenarios (Van Nguyen et al., 2022; Lu
et al., 2021), enables knowledge sharing across
multiple tasks and encourages the fine-tuned language model to generalize across a broader set of
text-based instruction scenarios. This is supported
by our results shown in Section 5.2 showing textto-schema outperforming conventional methods
