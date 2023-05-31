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


**[구현 및 성능]**   
* **구현**    
**DIRECT 접근 방식**으로 훈련된 기존 **meta-trained LM T0**(Sanh et al., 2021)과 **비교**하기 위해,     
**T0 대비 ~5%의 training compute만 사용**하여 20개의 서로 다른 데이터 세트(T0 훈련에 사용되는 데이터 세트의 약 절반)에서 **T5(Rafel et al., 2019) 모델을 Meta 훈련**하여 **FLIFFED를 구현**       
* **성능**       
BIG-Bench(Srivastava et al., 2022)의 14개 데이터 세트에 대한 평가는 FLIFFED가 효과적(Figure 2)      
모든 LM에 비해 SoTA을 보여줄 뿐만 아니라 훨씬 더 큰 GPT-3 175B(3샷)를 크게 능가       
또한 14개의 추가적인 일반적인 영어 NLP 작업에 대한 기준 모델과 FLIPPED를 비교하여 이전 방법 및 모델에 비해 그 효과를 더욱 확대함.      
<img width="264" alt="image" src="https://github.com/yerimoh/img/assets/76824611/c34edf36-ad2d-470d-901f-57737e1368e6">


**[성능 증명]**       
* 가설설정 & FLIFFED가 unseen tasks에서 strong zero-shot generalization ability를 보여준다고 가정하고 이를 증명하는 실험을 할 것임    
* 우리는 unseen labels에 대한 generalization 기능이 향상되었다는 가설을 실험하기위해,     
surface forms은 다르지만 의미는 동일한 다양한 레이블 쌍(예: yes/no vs agree/disagree)에서 평가함    
* **결과**   
  * FLIPPED가 T0-11B와 최대 +20%의 평균 F1 점수 성능 격차를 가지고 있음을 보여줌   
  ➡ FLIPPED LEARNING이 label generalization 기능을 크게 향상시킨다는 것을 나타냄      
  * 28개의 task 중 가장 성능 향상이 뚜렷한 task가 unseen labels datasets에 대한 evaluation이다     
  ➡ 가설을 더욱 강력하게 증명 가능    
  * FLIPPED LEARNING은 label을 생성하는 대신, **label에 FLIPPED LEARNING conditions이 있기 때문**에 label generalization을 피할 가능성이 높다.     
 ➡ label generalization가 개선되어 결과적으로 작업 일반화가 개선된다.      


**요약하자면, 본 논문의 contribution은 다음과 같다:**      
* input instance와 label의 연결이 주어지면 **task instruction의 likelihood을 계산**하는 새로운 meta-training 방법인 **FLIPPED LEARNING을 제안**함    
* unlikelihood loss을 추가하여 LM이 입력 인스턴스 레이블 대응에 따라 작업 명령을 생성하도록 합니다.

• BIG-Bench 벤치마크의 14개 데이터 세트에서 11B 크기의 FLIFFED(FLIPPED Learning을 통해 훈련된 LM)가 메타 훈련된 T0-11B를 평균 8.4% 포인트 능가할 뿐만 아니라 16배 더 큰 3샷 GPT-3를 9.7% 포인트 능가한다는 것을 보여줍니다. 14개의 추가 영어 NLP 작업을 평가할 때, FLIFFED는 평균적으로 모든 기준 모델을 능가하여 제안된 방법의 효과를 더욱 입증합니다.

• 우리는 플립드가 메타 훈련 중에 보이지 않는 레이블에 대한 일반화에 특히 효과적이며, 새로운 레이블 쌍에 대해 T0-11B를 평균 F1 점수 최대 20% 능가한다는 것을 보여줍니다.




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






















































