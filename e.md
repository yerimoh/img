One of the most powerful features of contextualized models is their dynamic embeddings for words in context, leading to state-ofthe-art representations for context-aware lexical semantics. In this paper, we present a
post-processing technique that enhances these
representations by learning a transformation
through static anchors. Our method requires
only another pre-trained model and no labeled
data is needed. We show consistent improvement in a range of benchmark tasks that test
contextual variations of meaning both across
different usages of a word and across different
words as they are used in context. We demonstrate that while the original contextual representations can be improved by another embedding space from either contextualized or
static models, the static embeddings, which
have lower computational requirements, provide the most gains




1 Introduction
Word representations are fundamental in Natural
Language Processing (NLP) (Bengio et al., 2003).
Recently, there has been a surge of contextualized
models that achieve state-of-the-art in many NLP
benchmark tasks (Peters et al., 2018; Devlin et al.,
2019; Liu et al., 2019b; Yang et al., 2019). Even
better performance has been reported from finetuning or training multiple contextualized models
for a specific task such as question answering (Devlin et al., 2019; Xu et al., 2020). However, little
has been explored on directly leveraging the many
off-the-shelf pre-trained models to improve taskindependent representations for lexical semantics.
Furthermore, classic static embeddings are often
overlooked in this trend towards contextualized
models. As opposed to contextualized embeddings
that generate dynamic representations for words
in context, static embeddings such as word2vec
(Mikolov et al., 2013) assign one fixed representation for each word. Despite being less effective in
capturing context-sensitive word meanings, static
embeddings still achieve better performance than
contextualized embeddings in traditional contextindependent lexical semantic tasks including word
similarity and analogy (Wang et al., 2019). This
suggests that static embeddings have the potential
to offer complementary semantic information to enhance contextualized models for lexical semantics.



We bridge the aforementioned gaps and propose
a general framework that improves contextualized
representations by leveraging other pre-trained contextualized/static models. We achieve this by using
static anchors (the average contextual representations for each word) to transform the original contextualized model, guided by the embedding space
from another model. We assess the overall quality
of a model’s lexical semantic representation by two
Inter Word tasks that measure relations between different words in context. We also evaluate on three
Within Word tasks that test the contextual effect
from different usages of the same word/word pair.
Our method obtains consistent improvement across
all these context-aware lexical semantic tasks. We
demonstrate the particular strength of leveraging
static embeddings, and offer insights on the reasons
behind the improvement. Our method also has minimum computational complexity and requires no
labeled data
