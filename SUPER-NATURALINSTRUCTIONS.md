# UPER-NATURALINSTRUCTIONS:  Generalization via Declarative Instructions on 1600+ NLP Tasks


# Abstract
**목적:**    
NLP 모델을 instructions과 함께 제공될 때 **_다양한_ unseen tasks**으로 일반화          

**방법:** 
* 1,616개의 다양한 NLP 작업과 전문가가 작성한 지침의 벤치마크인 SUPER-NATURALINSTRUCTIONS을 소개.      
우리의 컬렉션은  classification, extraction, infill
ing, sequence tagging, text rewriting, and text
 composition.을 포함하지만 이에 국한되지 않는 76개의 개별 task 유형을 다룸.      
* 이렇게 크고 다양한 task 모음을 통해 instructions과에 따라 cross-task generalization를 엄격하게 벤치마킹할 수 있음    
 즉, 모델이  subset of tasks에 대한 instructions을 따르도록 training하고, 나머지 unseen task에 대한 지침을 평가함      
* Tk-INSTRUCT build    
다양한 incontext instructions(일반 언어 작업 정의 또는 k-shot 예제)을 따르도록 train된  transformer      

**결과:**     
* 우리의 실험에 따르면 **Tk-INSTRUCT는 크기가 다소 작음에도 불구**하고,   
 Instruct GPT와 같은 기존 instruction-following 모델을 **벤치마크에서 9% 이상 능가**     
 
우리는 관찰된 작업 수, 작업당 인스턴스 수, 모델 크기와 같은 다양한 스케일링 매개 변수의 함수로서 일반화를 추가로 분석.   
우리는 우리의 데이터 세트와 모델이 더 범용적인 NLP 모델을 향한 미래의 발전을 촉진하기를 바란다.       


---
---
 
 
 # 1 Introduction

**[기존 문제]**            
* 특히 주요 모델 뒤에 있는 **기업에서 공개한 데이터가 제한적**이어서 supervised data의 역할은 여전히 미진한 상태로 남아 있다.     
* 게다가, 연구 커뮤니티가 이러한 **거대한 모델을 확장하고 재교육하는 것은 거의 불가능**합니다.      

➡ 이 두 가지 과제를 해결하려면, unseen tasks으로 일반화할 수 있는 모델을 개발하고 평가하기 위해 **광범위한 NLP 작업의 large-scale public benchmarks와 instructions을 사용할 수 있어야 함**          


**[본 논문의 해결]**         
* **해결 1)** 본 논문은, instructions과 함께 다양한 NLP 작업으로 구성된 **meta-dataset(SUP-NATINST)를 구성**             
* **해결 2)** instructions이 주어진 new task을 수행할 수 있는 모델을 train하여 **Instruct GPT(16배 더 많은 매개변수를 사용)를 능가**함.                

**[해결 1) dataset: SUPER-NATURALINSTRUCTIONS (SUP-NATINST for short)]**
* 우리의 데이터 세트인 SUP-NATINST는 1,616개의 NLP 작업과 해당 자연어 명령의 대규모 벤치마크임.     
* 55개 언어에 걸친 76개의 광범위한 작업 유형과 같은 다양한 task를 제공        
* 각 task는 **입력 텍스트를 task 출력에 매핑하기 위한 task 정의**와 원하는 또는 **원하지 않는 출력을 보여주기 위한 몇 가지 예제로 구성된 명령**과 쌍을 이룸 (Figure 1)     
* Figure 1: An example task from SUP-NATINST   
<img width="217" alt="image" src="https://github.com/yerimoh/img/assets/76824611/4cd3f109-f796-4981-be06-46b0cef76081">
* 이러한 task와 instructions 88명의 NLP 실무자가 공개 요청에 따라 제공합니다.     
이러한 기여는 품질을 보장하기 위해 여러 차례의 동료 검토 및 크라우드소싱 피드백을 거친 후 통합됨.      
* 이렇게 다양하고 대규모의 데이터를 보유하면 task를 훈련 및 테스트 세트로 신중하게 나누고 최첨단 방법이 이들에게 어떻게 수행되는지 체계적으로 연구 가능      
* 표 1과 그림 2는 관련 벤치마크와 비교하여 SUP-NATINST의 속성을 강조하며 벤치마크의 작업 및 교육 유형의 다양성을 강조합니다.


<img width="416" alt="image" src="https://github.com/yerimoh/img/assets/76824611/9f885db3-a4a5-41cd-b3b9-91975f09081c">
Table 1: A comparison of SUP-NATINST to a few notable datasets in the field. We obtain the number of tasks,
instructions, and task types of other datasets from their original paper. “–” indicates the fields are not applicable or
unknown. Standards for categorizing task types vary across different datasets (see Fig. 2). *PROMPTSOURCE does
not provide task type annotation for all their tasks, for which we report only the 13 task types annotated for training
T0 (Sanh et al., 2022) instead.
<img width="427" alt="image" src="https://github.com/yerimoh/img/assets/76824611/22d563d8-e4c7-43ff-b88e-4d94542ad7c8">
Figure 2: Compared to other datasets, SUP-NATINST covers a more diverse range of task types. InstructGPT reports
a very coarse categorization of their task types. Bubble size represents the number of tasks of each type in log scale.

Our model, Tk-INSTRUCT, is a generative model for transforming task inputs given declarative in-context instructions (task definition or kshot examples). It is built by multi-task training of the T5 model (Raffel et al., 2020) over all the task instructions in our training set, and is eval uated on unseen tasks in the test set. Interestingly, an 11B-parameter Tk-INSTRUCT can outperform the 175B-parameter InstructGPT model by 9.9 ROUGE-L points when evaluated on 119 unseen English tasks, and the multilingual variant mTk-INSTRUCT outperforms InstructGPT by 13.3 points on 35 non-English tasks (§6.1). According to human evaluation, Tk-INSTRUCT generates responses at least as well as the ground truth for 77% of the testing instances (§6.2), confirming its strong generalization to unseen tasks.

우리의 모델인 Tk-INSTRUCT는 선언적 인 컨텍스트 지침(작업 정의 또는 kshot 예제)이 주어진 작업 입력을 변환하기 위한 생성 모델입니다. T5 모델(Raffel et al., 2020)의 다중 작업 훈련에 의해 훈련 세트의 모든 작업 지침에 걸쳐 구축되며, 테스트 세트에서 보이지 않는 작업에 대해 평가됩니다. 흥미롭게도, 11B 매개 변수 Tk-INSTRUCT는 119개의 보이지 않는 영어 작업에서 평가될 때 175B 매개 변수 InstructGPT 모델을 9.9ROUGE-L 포인트만큼 능가할 수 있으며, 다국어 변형 mTk-INSTRUCT는 35개의 영어가 아닌 작업(6.1)에서 13.3 포인트만큼 InstruitruituteGPT를 능가합니다. 인간의 평가에 따르면, Tk-INSTRUCT는 테스트 인스턴스의 77%(§6.2)에 대한 응답과 함께 최소한의 응답을 생성하여 보이지 않는 작업에 대한 강력한 일반화를 확인합니다.

TkINSTRUCT의 강력한 경험적 성능은 일반화 가능한 NLP 모델에 대한 연구를 용이하게 하기 위해 SUP-NATINST와 같은 초대형 메타 데이터 세트의 중요성을 확인합니다. 우리는 이 일반화의 중요한 요소를 이해하기 위해 광범위한 분석을 수행합니다(§7). 우리의 분석은 훈련 작업의 다양성과 모델 크기를 확장하는 것이 보이지 않는 작업에 대한 강력한 일반화에 모두 중요하다는 것을 보여줍니다. 마지막으로 성능 상한을 추정하여 추가 개선의 여지를 제시합니다.

The compelling empirical performance of TkINSTRUCT confirms the importance of super-sized meta datasets such as our SUP-NATINST to facilitate research towards generalizable NLP models. We conduct extensive analysis to understand the important factors for this generalization (§7). Our analysis shows that scaling up the diversity of training tasks and the model size are both important for strong generalization to unseen tasks. Finally, we estimate performance upper bounds, suggesting further room for improvement.





 
 
 
 
 
