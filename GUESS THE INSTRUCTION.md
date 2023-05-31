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
**[previous work & problem]**       
* **previous work: meta-training(prompt)**        
   * 방대한 양의 **corpora에서 pretrain된** Large Language Models (LMs)은 **task-specific fine-tuning없이,**    
   **input instances**와 **instructions (task prompts)** 을 통해 다양한 다운스트림 작업을 해결가능     
   * meta-training이라고도 하는 prompted input (instruction and input)이 주어지면 정답을 생성하여,   
    다양한 다운스트림 작업에서 LM을 fine-tuning하면 zero-shot task generalization가 크게 향상된다는 것을 보여주었음   
* **meta-training의 문제점**    
   * 그러나 이후 연구는 이 표준 접근법을 통해 <span style="background-color:#fff5b1">**meta-trained LM**이 **different label words에 민감**</span>하다는 것을 보여주며,    
   이는 standard meta-trained LMs이 종종 <span style="background-color:#fff5b1">**novel labels을 포함하는 작업으로 generalize하지 못한다**는 것을 의미</span>한다.      


**[FLIPPED LEARNING]**        
* alternative meta-training method       
* **task instruction과 label space**을 **flips**하여 **input instance와 label이 주어졌을 때** <span style="background-color:#fff5b1">기본 LM이 **instruction을 생성하도록 훈련**함.</span>   
* 이는 standard meta-training methods와 다름        
  * **standard meta-training methods(DIRECT)**: LMs이 instruction 및 input instance가 주어졌을 때, **label를 생성**하도록 훈련    
  * **standard meta-training methods(CHANNEL)**: LMs이 label이 주어졌을 때, **instruction, input instance를 생성**하도록 훈련           
* 또한 FLIPPED Learning에 대한 **unlikelihood loss을 추가:** LM이 잘못된 label option에 대한 task instruction을 생성하지 않도록 함.       
* 추론 중에 FLIPPED Learning을 통해 훈련된 LM은 Figure 1:과 같이 **task instruction을 생성할 가능성이 가장 높은 label option을 선택**함
<img width="384" alt="image" src="https://github.com/yerimoh/img/assets/76824611/8cecf8f0-1651-4446-8027-ce7aa9c3e711">


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






















































