The
NER task requires models to extract summarylevel information from materials science text
and recognize entities including materials, descriptors, material properties, and applications
amongst others. The NER task predicts the
best entity type label for a given text span
si with a non-entity span containing a “null”
label. MatSci-NLP contains NER task data
adapted from Weston et al. (2019); Friedrich
et al. (2020); Mysore et al. (2019); Yamaguchi
et al. (2020).

• Relation Classification: In the relation classification task, the model predicts the most
relevant relation type for a given span pair
(si
, sj ). MatSci-NLP contains relation classification task data adapted from Mysore et al.
(2019); Yamaguchi et al. (2020); MatSciRE
(2022).


• Event Argument Extraction: The event
argument extraction task involves extracting
event arguments and relevant argument roles.
As there may be more than a single event for
a given text, we specify event triggers and
require the language model to extract corresponding arguments and their roles. MatSciNLP contains event argument extraction task
data adapted from Mysore et al. (2019); Yamaguchi et al. (2020).



 Paragraph Classification: In the paragraph
classification task adapted from Venugopal
et al. (2021), the model determines whether a
given paragraph pertains to glass science.


Synthesis Action Retrieval (SAR): SAR is
a materials science domain-specific task that
defines eight action terms that unambiguously
identify a type of synthesis action to describe
a synthesis procedure. MatSci-NLP adapts
SAR data from Wang et al. (2022a) to ask
language models to classify word tokens into
pre-defined action categories.

• Sentence Classification: In the sentence
classification task, models identify sentences
that describe relevant experimental facts based
on data adapted from Friedrich et al. (2020).



• Slot Filling: In the slot-filling task, models
extract slot fillers from particular sentences
based on a predefined set of semantically
meaningful entities. In the task data adapted
from Friedrich et al. (2020), each sentence describes a single experiment frame for which
the model predicts the slots in that frame.

