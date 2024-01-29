Task Descriptions5 We evaluate on three Within
Word tasks. Usage Similarity (Usim) (Erk et al.,
2013) dataset measures graded similarity of the
same word in pairs of different contexts on the
scale from 1 to 5. Word in Context (WiC) (Pilehvar
and Camacho-Collados, 2019) dataset challenges a
system to predict a binary choice of whether a pair
of contexts for the same word belongs to the same
meaning or not. We follow the advised training
scheme in the original paper to learn a cosine similarity threshold on the representations. The recently
proposed CoSimlex (Armendariz et al., 2019) task
provides contexts for selected word pairs from the
word similarity benchmark SimLex-999 (Hill et al.,
2015) and measures the graded contextual effect.
We use the English dataset from this task. Its first
subtask, CoSimlex-I, evaluates the change in similarity between the same word pair under different
contexts. As it requires a system to capture different contextual representations of the same word
in order to correctly predict the change of similarity to the other word in the pair, CoSimlex-I
indirectly measures within-word contextual effect
and therefore provides our third Within Word task.
The second CoSimLex subtask, CoSimlex-II, is an
Inter Word task as it requires a system to predict
the absolute gold rating of each word pair consisting of different words in each context. We also
evaluate on another related Inter Word task, Stanford Contextual Word Similarity (SCWS), which
provides graded similarity scores of word pairs in
independent contexts. Compared with the two Inter Word tasks, the three Within Word tasks are
more sensitive to contextual effects since they penalize strongly a static model (eg. FastText) as
being no better than a random baseline. By contrast, we might expect a context-independent static
model to perform reasonably, though not as good
as a context-sensitive model, in InterW ord tasks
(Armendariz et al., 2019).


Results: Table 1 reports the performance of each
contextualized model before and after the transformation guided by each of the other contextual/static
embeddings. In this table, → indicates the direction of the transformation. For example, RoBERTa
→ FastText denotes using FastText as the target
space to transform RoBERTa



We find that applying transformation is generally able to improve each contextualized model,
obtaining the best performance across all the tasks.
In particular, we observe substantial improvements
in Usim (ca. 0.04 increase of ρ) and SCWS (ca.
0.03 increase of ρ). The most consistent improvement comes from leveraging static embeddings.
This is especially evident in Inter Word tasks where
transforming towards FastText achieves the best
performance but leveraging another contextualized
model often brings harm. This suggests that the
static embeddings are able to inject better inter
word relations (Wang et al., 2019) into a contextualized model. At the same time, static embeddings
consistently improve performance in Within Word
tasks in 24 out of the total 27 configurations, reassuring us that the contextualization power of the
original contextual space is not only preserved but
even enhanced. Overall, FastText is the most robust
target space as it improves all the contextualized
source representations for all the tasks except for
XLNet in WiC. SGNS and GloVe are also competitive especially in improving Within Word tasks.


Analysis: The overall improvement in both Within
Word and Inter Word tasks suggests two possible
benefits from the transformation: better withinword contextualization and better overall interword semantic space. We perform controlled studies that test for these two sources of improvement
in isolation. We test on the best base contextualized
space (RoBERTa) with the various transformations.


The fact that a static embedding (FastText) performs better than a random baseline in Within Word
tasks (see Table 1) suggests that there are some
lexical cues in the target words (eg. morphological
variations) that can help solve the task alongside
the context. To highlight the improvement in contextualization alone, since the Within Word tasks
before lemmatization may contain different word
forms of the same lemma as the target words in
each pair, we lemmatize all the target words in
the dataset. As a result, each pair in the Within
Word tasks now contains the identical target word.
We observe that the results after lemmatization are
slightly lower than before but the transformation
especially towards static embeddings is indeed able
to improve the contextualization across all the tasks
(Table 2).



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




