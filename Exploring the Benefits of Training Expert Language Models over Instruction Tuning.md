# Exploring the Benefits of Training Expert Language Models over Instruction Tuning


# Abstract
       
최근 multitaskprompted fine-tuning (MT)이라고도 하는 여러 작업에 조정된 Language Models (LMs) instructiontuned은 unseen tasks로 generalize 기능을 보여주었다. 


**[Problem of Previous work]**     
* **training tasks의 수를 확장**하는 것이 **더 강력한 MT LMs을 만드는 핵심 구성 요소**임을 보여주었다.       
* ⚠️ 우리는 **a single task에 fine-tuned된 expert LM**이 매우 좋은 결과를 내는 위 가설을 부정할 만할 결과 발견   
➡ <span style="background-color:#fff5b1">이 발견은 **단순히 작업의 수를 증가**시키는 것이 **더 강력한 MT LM을 만든다**는 기존의 믿음에 **의문**을 던짐</span>     

**[본 논문의 main idea]**     
* 이 발견을 활용하여, 또한 zero-shot inference을 위한 single MT LM 대신,     
**training task당 별도의 expert LM train시키는 분산 접근 방식**은     
    * **(1)** instruction tuning 중 종종 발생하는 **negative task transfer을 피하는 것**을 포함하여 많은 이점을 가지고 있음을 보여준다       
    * (2) catastrophic forgetting을 피하기 위해 previous tasks를 재교육하지 않고도 **새로운 tasks를 지속적으로 학습 가능**     
    * (3) 개별 experts를 통합할 때 구성 능력(compositional capabilities)을 보여줌      
* 코드는 [링크](https://github.com/joeljang/ELM)에서 확인할 수 가능    



----





# 1. Introduction
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



































