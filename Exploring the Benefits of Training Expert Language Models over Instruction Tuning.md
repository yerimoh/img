# Exploring the Benefits of Training Expert Language Models over Instruction Tuning


# Abstract
Recently, Language Models (LMs) instructiontuned on multiple tasks, also known as multitaskprompted fine-tuning (MT), have shown the capability to generalize to unseen tasks. Previous work
has shown that scaling the number of training
tasks is the key component in making stronger MT
LMs. In this work, we report an unexpected finding that an expert LM fine-tuned on just a single
task can outperform an MT LM trained with 300+
different tasks on 11 different unseen datasets and
on 13 datasets of the BIG-bench benchmark by
a mean accuracy of 3.20% and 1.29%, respectively. This finding casts doubt on the previously
held belief that simply scaling the number of tasks
makes stronger MT LMs. Leveraging this finding,
we further show that this distributed approach of
training a separate expert LM per training task instead of a single MT LM for zero-shot inference
possesses many benefits including (1) avoiding
negative task transfer that often occurs during
instruction tuning, (2) being able to continually
learn new tasks without having to re-train on previous tasks to avoid catastrophic forgetting, and (3)
showing compositional capabilities when merging
individual experts together. The code is available
at https://github.com/joeljang/ELM.



----





1. Introduction
Recent works show pretrained Language Models (LMs) that
have been fine-tuned on multiple tasks with instructions
(prompted instances), also known as multitask-prompted
fine-tuned LMs and referred to as MT LMs in this work,
can generalize to unseen tasks without task-specific finetuning (Wei et al., 2021; Sanh et al., 2021; Chung et al
2022; Ye et al., 2022b; Ouyang et al., 2022; Wang et al.,
2022a; Muennighoff et al., 2022). This paper raises some
questions regarding the current paradigm of training MT
LMs and is mainly divided into two parts. In Part 1, we
report an unexpected finding regarding expert LMs (trained
only on a single task) compared to MT LMs. In Part 2, we
leverage the finding to highlight some of the benefits of
expert LMs over MT LMs.



Part 1 (Section 5) Previously, the key component to enhancing the unseen task generalization performance of MT
LMs was thought to be scaling the total number of tasks
used in training (Wei et al., 2021; Chung et al., 2022; Wang
et al., 2022a). However, in this work, we show that training
a single expert LM on one1 out of the 300+ tasks used to
train an MT LM (T0-3B (Sanh et al., 2021)) can outperform
the MT LM by a non-trivial margin on 24 unseen tasks on
mean accuracy.


Specifically, following the same experimental setup (training and evaluation) as T0-3B (Sanh et al., 2021), one of the
most widely used MT LM, we first train expert LMs for
each given training task (296) by freezing the underlying
LM and updating adapters (Houlsby et al., 2019). We report
a finding that shows 7 out of the 296 experts surpass T0-
3B on the capability to generalize to unseen tasks on mean
accuracy (shown in Figure 1). Using the top performing
expert for all of the unseen task evaluation tasks surpasses
T0-3B by a mean accuracy of 3.20% and 1.29% on 11 unseen datasets and 13 datasets of the BIG-Bench benchmark,
respectively. We also show that applying a simple mechanism to retrieve relevant experts for each individual unseen
task results in comparable performance to T0-3B. Considering the significant room for improvement when retrieving
the best-performing expert for each unseen task (+11.94%
compared to T0-3B), these results imply that choosing the
right expert rather than na¨ıvely utilizing a single MT LM for
all of the unseen tasks can be a more efficient and effective
approach





Part 2 (Section 6) Leveraging the finding of expert LMs
showing improved unseen task generalization capability, we
highlight three other advantages of training multiple expert
LMs for each task and retrieving the relevant expert during
inference (shown in Figure 2) compared to training MT
LMs.




#1. MT LMs do not show the optimal performance for seen
tasks because of negative task transfer, where learning multiple tasks at once hinders the learning of some specific
tasks (Aghajanyan et al., 2021; Asai et al., 2022a; Zhang
et al., 2022). Expert LMs, on the other hand, are not subject
to negative task transfer (Levine et al., 2022) since each task
is learned independently; We show our approach of selecting relevant experts during inference results in a +10.4%
mean accuracy improvement on validation datasets of the
36 training tasks compared to T0-3B.


#2. MT LMs are susceptible to catastrophic forgetting (Mc
Closkey & Cohen, 1989) of previous tasks when learning
new tasks and require re-training on previous tasks to mitigate forgetting (Chakrabarty et al., 2022). Results show our
distributed (training individual tasks in an independent manner) approach results in absolutely no degradation of seen
tasks, even when adding the 8 new experts to the Expert
Library, without re-training on previous tasks when learning
8 new generative tasks.



#3. We show that MT LMs show poor ability in performing
composition of previously learned tasks given via concatenation of the corresponding instructions as a single compositional instruction. On the other hand, we show that
merging the two experts trained on the individual tasks with
mT5-3B (Xue et al., 2021) as the underlying pre-trained
LM results in an expert that can outperform its MT LM
counterpart, mT0-3B (Muennighoff et al., 2022), by a mean
ROUGE-L score of +2.71 on 5 novel compositional tasks
(summarization & translation). Details of the merging mechanism are provided in Section 3.3.



































