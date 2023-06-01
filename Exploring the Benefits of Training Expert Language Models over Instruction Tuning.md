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
    * **(2)** catastrophic forgetting을 피하기 위해 previous tasks를 재교육하지 않고도 **새로운 tasks를 지속적으로 학습 가능**     
    * **(3)** 개별 experts를 통합할 때 구성 능력(compositional capabilities)을 보여줌      
* 코드는 [링크](https://github.com/joeljang/ELM)에서 확인할 수 가능    



----





# 1. Introduction

**Recent works:**      
최근의 연구는 multitask-prompted fine-tuned(본 연구에서MTLM라고 부름)가 multiple tasks(prompted instances)으로 여러 task에 대해 pretrained Language Models(LMs)이 task-specific finetuning 없이 unseen task로 generalize될 수 있음을 보여줌

**본 논문의 구성**          
본 논문은 **MTLM을 훈련하는 현재 패러다임과 관련하여 몇 가지 질문을 제기**하며 주로 두 부분으로 나뉨      
* **Part 1:** MT LM과 비교하여 expert LM(a single task)과 관련하여 예상치 못한 발견을 보고      
    * **single expert LM**을 MTLM에 사용한 300이상의 task 중 1개에 대해 훈련하면,     
unseen task인 24개의 작업에서 사소한 차이로 MTLM을 능가할 수 있음을 보여줌.     
➡ zero-shot inference을 위한 single MT LM 대신, **training task당 별도의 expert LM train시키는 분산 접근 방식**이 더 좋음    
➡ 모든 task에 대해 단일 MT LM을 na¨ıvely하게 활용하는 것보다 올바른 전문가를 선택하는 것이 더 효율적이고 효과적인 접근 방식이 될 수 있음을 의미      
* **Part 2:** MT LM에 비해 **expert LM의 이점**을 일부 강조하기 위해 이를 활용함      
3가지 이점 강조    
    * **1)** 
    **MT LMs:** negative task transfer때문에 seen tasks에 대한 optimal performance 내지 못함    
    **Expert LMs:** 각 **task가 독립적으로 학습**되어, negative task transfer이 없어 **성능이 더 좋음**            
    * **2)** 
    **MT LMs:** new tasks를 train할 때, catastrophic forgetting에 취약, 이를 완화하기위해 re-training on previous tasks 필요    
    **Expert LMs:** re-training 없이도 좋은 성능을 보여줌   
    * **3)**     
    **MT LMs:** a single compositional instruction로 해당 instruction의 연결을 통해 composition of previously learned tasks의 구성을 수행하는 능력이 떨어짐    
    **Expert LMs:** 이를 mT5-3B(Xue et al., 2021)와 결합하면, 5 novel compositional tasks (summarization & translation)에 대해 좋은 성능을 보임   
    Details of the merging mechanism are provided in Section 3.3     
    
Cherry-picked
output examples of the MT LM and the merged experts are
provided in Table 8.


<img width="239" alt="image" src="https://github.com/yerimoh/img/assets/76824611/86ee64eb-f53b-4635-b1e2-90cbe1e34d9f">


<img width="431" alt="image" src="https://github.com/yerimoh/img/assets/76824611/56a55096-106c-4980-a298-56c88913fabc">


------


# 7. Limitations and Discussions
* 우리는 instruction 튜닝의 주요 단점 중 일부를 강조하고 이 논문에서 대신 전문가를 교육하고 검색하는 대안적 접근법을 제안하지만, 우리는 **>11B 매개 변수를 가진 MTLMS에 대해 실험 결과를 수행하지 않음**      
예를 들어, >11B 매개 변수가 있는 MT LM은 모델 용량이 증가하기 때문에 negative task transfer에 덜 취약할 수 있름.      
* 또한 unseen tasks을 inference하는 동안, 우리의 검색 메커니즘은 배치 추론(즉, 레이블 없이 대상 작업의 32개 샘플에 액세스할 수 있음)을 가정     
* 마지막으로, compositional instruction 실험을 보여줄 때, 우리는 평가 인스턴스와 함께 입력으로 주어진 compositional instruction(보이는 두 명령의 연결)에서 두 명의 optimal experts를 검색할 수 있다고 가정합니다. 이것은 별도의 분해 단계가 필요할 수 있는 더 복잡하고 구성적인 명령어의 경우에는 반드시 그렇지 않을 수 있음    
* 대신 우리는 experts를 병합하는 것이 향후 작업을 위해 추론하는 동안 optimal experts를 검색하는 새로운 방법을 개발하고 남길 수 있는 가능성을 보여주는 데 중점을 둡니다.    

# 8. Conclusion
* 우리는 **single tasks**에 대해 **훈련된 expert LMs**이 **unseen tasks에 대해 strong generalization capability**을 보여주며,         
심지어 **multiple tasks (300+)** 에 대해 trained MT LMs을 **사소한 차이로 능가**한다는 흥미로운 발견을 제공      
* 우리는 이 기능을 활용하고 MT LMs을 통한 inference을 위해 **experts**를 교육하고 retrieving하는 **세 가지 주요 이점**을 보여줌    
    * 제안된 분산 접근 방식이 negative task transfer에 대해 더 강력      
    * new tasks을 학습하는 데 더 잘 적응      
    * compositional instructions을 수행할 수 있음        
* 이를 위해, 우리는 본 논문에서 명시적으로 탐구하지 않은 효율성, 개인 정보 보호 및 개인화(efficiency, privacy, and personalization)를 포함한 다른 미래 이점을 가질 수 있는 experts의 분산 및 협업 교육을 research community가 추가로 탐구할 것을 촉구함      



