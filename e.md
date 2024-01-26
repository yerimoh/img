Our method3
is built from a recently proposed
cross-lingual alignment technique called meeting
in the middle (Doval et al., 2018). Their method
relies on manual translations to learn a transformation over an orthogonal alignment for better
cross-lingual static embeddings. We show that
by a similar alignment + transformation technique,
we can improve monolingual contextualized embeddings without resorting to any labeled data.


The direct correspondence among contextualized and static embeddings for alignment is not
straightforward, as contextualized models can compute infinite representations for infinite contexts.
Inspired by previous study (Schuster et al., 2019)
that found contextualized embeddings roughly
form word clusters, we take the average of each
word’s contextual representations as anchors of a
contextualized model. We call them static anchors
as they provide one fixed representation per word,
and therefore correspond to word embeddings from
a static model such as FastText. We also use these
anchors to align between contextualized models.
To form the vocabulary for creating static anchors
in our experiments, we take the top 200k most frequent words and extract their contexts from English
Wikipedia.

To describe the method in more detail, we represent the anchor embeddings from the original contextualized model as our source matrix S, and the
corresponding representations from another contextualized/static model as target matrix T. si and
ti are the source and target vectors for the ith word
in the vocabulary (V ). We first find an orthogonal
alignment matrix W that rotates the target space to
the source space by solving the least squares linear
regression problem in Eq. 1. W is found through
Procrustes analysis (Schonemann ¨ , 1966).


As described in Eq. 2, we then learn a linear
mapping M to transform the source space towards
the average of source and the rotated target space,
by minimizing the squared Euclidean distance between each transformed source vector Msi and the
mean vector µi
(µi = (si + Wti)/2). M is the
mapping we will use to transform the original contextualized space. Following Doval et al. (2018),
M is found via a closed-form solution.






For improved alignment quality, as advised by
Artetxe et al. (2016), we normalize and meancenter4
the embeddings in S and T a priori.
