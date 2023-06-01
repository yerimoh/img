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




------


7. Limitations and Discussions
While we highlight some of the major drawbacks of instruction tuning and propose an alternative approach of instead
training and retrieving experts in this paper, we do not perform experimental results over MT LMS that have more
than >11B parameters. For example, MT LMs with >11B
parameters may be less susceptible to negative task transfer because of increased model capacity. Also, during the
inference of unseen tasks, our retrieval mechanism assumes
batch inference (i.e. having access to 32 samples of the target
tasks without labels). Finally, when showing the compositional instruction experiments, we assume the two optimal
experts could be retrieved from the compositional instruction (concatenation of the two seen instructions) given as
the input along with the evaluation instance. This might not
necessarily be the case with more complex, compositional
instructions, which might require a separate decomposition
stage. We instead focus on showing the possibility merging
experts can bring and leave developing novel methods of
retrieving the optimal experts during inference for future
work.



# 8. Conclusion
In this work, we provide an interesting finding that expert
LMs trained on single tasks show strong generalization capability to unseen tasks, even surpassing MT LMs trained on
multiple tasks (300+) by a non-trivial margin. We leverage
this capability and show three main benefits of training and
retrieving experts for inference over MT LMs, demonstrating that our proposed distributed approach is more robust
against negative task transfer, more adapt at learning new
tasks, and can perform compositional instructions. To this
end, we urge the research community to further explore
distributed and collaborative training of experts which may
have other future benefits including efficiency, privacy, and
personalization not explicitly explored in this paper.




