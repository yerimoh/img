# GUESS THE INSTRUCTION!: FLIPPED LEARNING MAKES LANGUAGE MODELS STRONGER ZERO-SHOT LEARNERS


# ABSTRACT
**[Meta-training이란?]**             
* **정의**     
task instruction 및 input instance가 주어진 target label의 가능성을 최대화하여 다양한 다운스트림 작업에서 언어 모델(LM)을 fine-tunes          
보통 좋은 meta-learning model은 train time동안 접하지 않았던 새로운 task나 environment에 대해 잘 적응하거나, generalization을 잘한다는 것임     
* **효과**   
Meta-training은 **generation성능을 향상**시킴       
* **한계**     
meta-trained LMs은 여전히 **meta-training 중에 보이지 않는 새로운 레이블을 포함하는 task**를 **generalize**하는 데 어려움을 겪고 있음.       


**[본 논문의 제안]**    
* **FLIPPED LEARNING 제안**          
   * **input instance와 label이 주어진 task instruction을 생성**하도록 LM을 훈련시키는 **meta-training의 대안적인** 방법     
   * inference 중에 FLIPPED Learning으로 훈련된 LM은 **task instruction을 생성할 가능성이 가장 높은 label option을 선택**함.      
* **성능**    
   * BIG-bench benchmark의 14개 작업에서 11B-sized FLIPPED는  zero-shot T0- 11B(Sanh et al., 2021)와 심지어 16배 더 큰 3-shot GPT-3(175B)(Brown et al., 2020)를 각각 평균 8.4%, 9.7% 포인트 능가함       
   * FLIFFED는 **unseen labels 작업에서 특히 큰 성능 향상** (T0-11B를 최대 +20% 평균 F1 점수까지 능가)      
   * 이는 FLIPPED의 강력한 task generalization가 향상된 generalization에서 새로운 labels로 온다는 것을 나타냄        
* **코드**        
[링크](github.com/seonghyeonye/Flipped-Learning) 에서 코드를 공개함   




----


# 1 INTRODUCTION
Large Language Models (LMs) pretrained on a vast amount of corpora are capable of solving various
downstream tasks through instructions (task prompts) concatenated with the input instances without
any task-specific fine-tuning (Brown et al., 2020; Rae et al., 2021; Chowdhery et al., 2022; Zhang
et al., 2022). Previous work has shown that fine-tuning the LM on various downstream tasks by
generating the correct answer given a prompted input (instruction and input), also referred to as
meta-training, leads to significant improvement in zero-shot task generalization (Sanh et al., 2021;
Wei et al., 2021; Wang et al., 2022). However, Webson & Pavlick (2021); Min et al. (2022c) show
that LMs meta-trained through this standard approach are sensitive to different label words, implying
that standard meta-trained LMs often fail to generalize to tasks that contain novel labels.


In this paper, we introduce an alternative meta-training method called FLIPPED LEARNING that flips
the task instruction and label space, training the underlying LM to generate the instruction when
given the input instance and label. This differs from the standard meta-training methods which train
the LM to generate the label given instruction and input instance (DIRECT) or generate instruction
and input instance given the label (CHANNEL). Also, we add an unlikelihood loss for FLIPPED
LEARNING, making the LM not generate the task instruction for an incorrect label option. During
inference, the LM trained via FLIPPED LEARNING, referred to as FLIPPED, selects the label option
that is most likely to generate the task instruction, as shown in Figure 1.


To compare with an existing meta-trained LM T0 (Sanh et al., 2021) trained by the DIRECT approach, we implement FLIPPED by meta-training the T5 (Raffel et al., 2019) model on 20 different
datasets (around half of the datasets used to train T0) with only ∼5% of training compute compared to T0. Evaluation on 14 datasets from BIG-Bench (Srivastava et al., 2022) demonstrate that
FLIPPED is effective (Figure 2), not only showing state-of-the-art performance compared to all LMs
regardless of size in the zero-shot setting, but also outperforming much larger GPT-3 175B (3-shot)
by a significant margin, even without any demonstrations of the task (zero-shot). We also compare
FLIPPED with baseline models on 14 additional common English NLP tasks, further amplifying its
effectiveness compared to previous methods and models.




We hypothesize that FLIPPED shows strong zero-shot generalization ability on unseen tasks because of the improved generalization capability to unseen labels. To test this hypothesis, we evaluate on various label pairs with different surface forms but with the same meaning (e.g. yes/no vs
agree/disagree). Results show FLIPPED has up to +20% average F1 score performance gap with T0-
11B, indicating that FLIPPED LEARNING indeed significantly improves label generalization capability. This hypothesis is further bolstered by the fact that the tasks that show significant performance
improvement from the baselines among the 28 evaluation datasets are datasets with unseen labels
during meta-training. Because FLIPPED LEARNING conditions on the label instead of generating it,
FLIPPED LEARNING is likely to avoid label overfitting, resulting in improved label generalization,
which consequently leads to better task generalization.




In summary, our contributions are as follows:     
• We propose FLIPPED LEARNING, a novel meta-training method that computes the likelihood of the task instruction given the concatenation of input instance and label. By adding
an unlikelihood loss, we make LMs generate the task instruction depending on the input
instance-label correspondence.

• On 14 datasets from the BIG-Bench benchmark, we show that 11B-sized FLIPPED (LM
trained through FLIPPED LEARNING) outperforms not only meta-trained T0-11B by 8.4%
points on average, but also 16x larger 3-shot GPT-3 by 9.7% points. When evaluating on
14 additional English NLP tasks, FLIPPED outperforms all baseline models on average,
further demonstrating the effectiveness of our proposed method.

• We show that FLIPPED is particularly effective on generalization to labels that are unseen
during meta-training, outperforming T0-11B by up to 20% average F1 score for novel label
pairs.






















































