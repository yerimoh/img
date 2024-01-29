To highlight the improvement in contextualization alone, since the Within Word tasks
before lemmatization may contain different word
forms of the same lemma as the target words in
each pair, we lemmatize all the target words in
the dataset. As a result, each pair in the Within
Word tasks now contains the identical target word.
We observe that the results after lemmatization are
slightly lower than before but the transformation
especially towards static embeddings is indeed able
to improve the contextualization across all the tasks
(Table 2)

To test solely the effect on the overall inter word
semantic space of the contextualized model becomes better after the transformation, we ‘decontextualize’ the model by evaluating only on the
static anchors of the contextualized embeddings.
These static anchors are not sensitive to a particular context and can thus only reflect overall inter
word semantic space like embeddings from a static
model. We observe improvement from the transformation on the static anchors in Inter Word tasks.
In particular, aligning towards FastText brings the
largest and the most consistent gains. This suggests
that FastText may have offered a better ensemble
space with RoBERTa and results in a better overall
inter word semantic space.


To better understand the contextualization improvement in the Within Word scenario, we focus
on WiC to perform more detailed analysis. While
we report results on the test set in Table 1, we
present the following analysis on the train and development sets because the test set labels have not
been released. We focus on the best performing
model in the task, RoBERTa → SGNS, to examine
the difference before and after the transformation.


Overall, we observe a trend for the within-word
contextual representations to move slightly closer
to each other after the transformation, as the mean
cosine similarity of a pair’s contextual word representations across all instances has increased from
0.516 to 0.542. We further break down cases according to their labels and find that the transformation mainly brings the representations closer for
TRUE pairs (where the context pairs of the word
are indeed expressing the same meaning) with the
mean cosine similarity increased from 0.606 to
0.651. For FALSE cases where the context pairs
refer to distinct meanings of the target word, there
is less increase in similarity (from 0.426 to 0.433).
We also find that the improved performance in this
task can be largely attributed to correcting many
erroneous FALSE predictions in the original space
as these representations are drawn closer after the
transformation (See Appendix D). We qualitatively
examine these corrected TRUE cases (Examples
are provided in Appendix E), and found that the improvement typically comes from reduced variance
for the contextual representations of monosemous
words. An example is the word daughter. We
observe very low cosine similarity among its contextual representations in the original space. These
representations are drawn closer after the transformation (eg. cosine similarity from 0.48 to 0.67).
We suspect this might be related to contextualized
models’ over-sensitivity to context changes (Shi
et al., 2019).


To summarize the analysis, our controlled experiments confirm our two hypotheses that the
transformation brings two independent effects: improved overall inter-word semantic space and improved within-word contextualization. Our qualitative analysis shows that the improved within-word
contextualization is likely to be the result of context
variance reduction.
