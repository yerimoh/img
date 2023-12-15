Recent advances in pre-trained language models
have transformed the landscape of natural language
processing. Self-supervised pre-training objectives
including masked language modeling (Devlin et al.,
2019) and masked span infilling (Lewis et al., 2020)
enable pre-trained models to acquire linguistic (Hewitt and Manning, 2019; Manning et al., 2020) and
factual knowledge (Petroni et al., 2019) by modeling the distribution of naturally occurring texts.
However, most of these objectives are limited to exploiting the surface form of human language, and
the lack of grounded supervision calls into question
how well these representations can ever capture
meaning (Bender and Koller, 2020), not to mention
the underlying commonsense knowledge which is
often reasoned implicitly and does not appear in
the surface form of human language (Merrill et al.,
2021; Zhou et al., 2020a; Hwang et al., 2021). On
the other hand, commonsense reasoning is important for building generalizable models because it
enables the model to reason about a great number of events, causes, and effects, while observing
only a small fraction of them. The ineffectiveness
of self-supervised language model pre-training on
acquiring commonsense knowledge makes them
require a relatively large number of labeled examples to succeed in a downstream task and prune to
overfit task-specific correlations (Tu et al., 2020).


Therefore, equipping pre-trained language models with commonsense reasoning ability has at
tracted much attention. To this end, two distinct
lines of research focus on improving commonsense
reasoning ability of pre-trained language models.
The first one focuses on incorporating external commonsense knowledge graph for commonsense reasoning (Lin et al., 2019; Liu et al., 2021; Cui and
Chen, 2021) while the other attempts to inject commonsense knowledge into the parameters of pretrained models (Li et al., 2019; Zhou et al., 2021;
Klein and Nabi, 2021). In this work we focus on
the second type of method because it alleviates the
need for external knowledge bases for training and
inference on downstream tasks, thus simpler, more
efficient, and not limited by the coverage issue of
external knowledge bases.


Prior work injects commonsense knowledge into
pre-trained models either on symbolic commonsense knowledge graphs with manually defined
rules (Li et al., 2019) or masked language modeling (Hosseini et al., 2021) or on general text
corpus with concept-centric self-supervised objectives (Zhou et al., 2021). The former method is
limited by the coverage of knowledge graphs and
human-written rules. It also fails to make use of
large-scale diverse natural text corpus. Therefore,
the training is limited to short and synthetic commonsense tuples, which affects its generalization
ability on diverse downstream tasks. The latter
method, however, only captures surface-level order relations between concepts and fails to learn
commonsense relations between concepts such as
cause, effect, intent, requirement, etc., which are
crucial for commonsense reasoning but often implicitly reasoned, thus do not appear in the surface
form of natural language.



In this work, we propose commonsense knowledge transfer, an alternative framework to refine a
general purpose pre-trained model’s commonsense
reasoning ability. In contrast to previous work,
it aims to transfer the commonsense knowledge
stored in a neural commonsense knowledge model
(e.g., COMET (Bosselut et al., 2019)) to a general
purpose pre-trained model on large scale general
text corpus. In this way, our approach combines the
best of both worlds from prior art: the dense and
informative commonsense knowledge from commonsense knowledge graphs and the accessibility
of large-scale diverse general corpus.



Commonsense knowledge transfer is conceptually related to knowledge distillation (KD) (Hinton et al., 2015) since they both aim to trans
fer knowledge from a knowledge-rich model to
another model that lacks it. However, different
from conventional KD, in commonsense knowledge transfer, the source model (i.e., neural commonsense model) and the target model (i.e., pretrained model) are heterogeneous. Moreover, instead of simply mimicking the teacher model, commonsense knowledge transfer requires the target
model to learn specialized knowledge from the
source model while retaining its own capability.
This poses unique challenges since the knowledge
transfer can not be accomplished by simply matching the logits or feature distribution between the
student and the teacher. To this end, we propose
to first extract commonsense knowledge in textual
form from the source model and then exploit the
extracted knowledge to form self-supervised training data for the target model. As illustrated in
Figure 1, commonsense knowledge transfer first
exploits general texts to form queries for retrieving commonsense knowledge from the neural commonsense knowledge model. Then it refines a pretrained model with two self-supervised objectives
that align the surface form of human language with
its underlying commonsense inference: commonsense text infilling and commonsense relation prediction. The former objective concatenates natural
text with its commonsense inference to form an
input example, masks certain spans in it, and trains
the model to reconstruct the original input. The latter method instead trains the model to distinguish
valid commonsense inference from carefully constructed spurious commonsense inference given the
original text and commonsense relation. Refining
a pre-trained model by multi-tasking on both generation (former) and understanding (latter) tasks
enables the model to better adapt to different kinds
of downstream tasks.



We refine T5 (Raffel et al., 2020) with commonsense knowledge transfer and fine-tune the resulting model downstream tasks requiring commonsense reasoning ability in both the fully supervised
setting and few-shot settings where only a percentage of labeled examples are available. Experimental results show substantial improvements in downstream tasks requiring commonsense reasoning, especially in the few-shot setting, demonstrating the
effectiveness of our approach.
