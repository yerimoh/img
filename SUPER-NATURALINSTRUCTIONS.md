# UPER-NATURALINSTRUCTIONS:  Generalization via Declarative Instructions on 1600+ NLP Tasks


# Abstract
 How well can NLP models generalize to a va
riety of unseen tasks when provided with task
 instructions? To address this question, we first
 introduce SUPER-NATURALINSTRUCTIONS,1
 a benchmark of 1,616 diverse NLP tasks and
 their expert-written instructions. Our collec
tion covers 76 distinct task types, including but
 not limited to classification, extraction, infill
ing, sequence tagging, text rewriting, and text
 composition. This large and diverse collec
tion of tasks enables rigorous benchmarking of
 cross-task generalization under instructions—
 training models to follow instructions on a sub
set of tasks and evaluating them on the remain
ing unseen ones.
 Furthermore, we build Tk-INSTRUCT, a trans
former model trained to follow a variety of in
context instructions (plain language task defi
nitions or k-shot examples). Our experiments
 show that Tk-INSTRUCT outperforms existing
 instruction-following models such as Instruct
GPTbyover9%onourbenchmark despite be
ing an order of magnitude smaller. We further
 analyze generalization as a function of various
 scaling parameters, such as the number of ob
served tasks, the number of instances per task,
 and model sizes. We hope our dataset and
 model facilitate future progress towards more
 general-purpose NLP models.2
 
 
 
 # 1 Introduction
The NLP community has witnessed great progress
 in building models for generalization to unseen
 tasks via in-context instructions using
 large pretrained language models (Raffel et al.,
 2020; Brown et al., 2020). As remarkable as mod
els like InstructGPT (Ouyang et al., 2022) are, the
 contribution of various design choices to their suc
cess is opaque. In particular, the role of super
vised data has remained understudied due to lim
ited data released by the corporate entities behind
 major models. In addition, it is nearly impossible
 for the research community to extend and re-train
 these gigantic models. Addressing these two cha
 lenges necessitates the availability of large-scale
public benchmarks of a broad range of NLP tasks
and their instructions to facilitate developing and
evaluating models that can generalize to unseen
tasks
 
 
 In this paper, we construct a meta-dataset (i.e.,
dataset of datasets; Triantafillou et al., 2019) that
consists of a wide variety of NLP tasks with their
instructions, and train a model that can perform
a new task given the instruction, outperforming
InstructGPT (which uses 16× more parameters).

Our dataset, SUPER-NATURALINSTRUCTIONS
(SUP-NATINST for short), is a large benchmark of
1,616 NLP tasks and their natural language instructions. It brings in a diverse variety of tasks—76
broad task types spanning 55 different languages.
Each task is paired up with an instruction that consists of the task definition for mapping an input text
to a task output and several examples for demon
strating the desired or undesired output (see Fig.1
as an example task). These tasks and their instructions are contributed by 88 NLP practitioners, in
response to our public call. These contributions are
consolidated after several rounds of peer-review
and crowdsourced feedback to ensure quality. Having this diverse and large-scale data enables us
to carefully split the tasks into training and test
sets and systematically study how state-of-the-art
methods perform on them. Table 1 and Figure 2
highlight properties of SUP-NATINST compared to
relevant benchmarks, emphasizing the diversity of
tasks and instruction types in our benchmark.


Our model, Tk-INSTRUCT, is a generative
model for transforming task inputs given declarative in-context instructions (task definition or kshot examples). It is built by multi-task training
of the T5 model (Raffel et al., 2020) over all the
task instructions in our training set, and is eval
uated on unseen tasks in the test set. Interestingly, an 11B-parameter Tk-INSTRUCT can outperform the 175B-parameter InstructGPT model
by 9.9 ROUGE-L points when evaluated on 119
unseen English tasks, and the multilingual variant
mTk-INSTRUCT outperforms InstructGPT by 13.3
points on 35 non-English tasks (§6.1). According
to human evaluation, Tk-INSTRUCT generates responses at least as well as the ground truth for 77%
of the testing instances (§6.2), confirming its strong
generalization to unseen tasks.


The compelling empirical performance of TkINSTRUCT confirms the importance of super-sized
meta datasets such as our SUP-NATINST to facilitate research towards generalizable NLP models.
We conduct extensive analysis to understand the
important factors for this generalization (§7). Our
analysis shows that scaling up the diversity of training tasks and the model size are both important
for strong generalization to unseen tasks. Finally,
we estimate performance upper bounds, suggesting
further room for improvement.


 
 
 
 
 
