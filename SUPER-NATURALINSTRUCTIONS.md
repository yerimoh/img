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
* **해결 1)** 본 논문은, instructions과 함께 다양한 NLP 작업으로 구성된 <span style="background-color:#FFE6E6">**meta-dataset(SUP-NATINST)를 구성**</span>            
* **해결 2)** instructions이 주어진 <span style="background-color:#FFE6E6">new task을 수행할 수 있는 **모델(Tk-INSTRUCT)**</span>을 train하여 **Instruct GPT(16배 더 많은 매개변수를 사용)를 능가**함.                

**[해결 1) dataset: SUPER-NATURALINSTRUCTIONS (SUP-NATINST for short)]**       
* 우리의 데이터 세트인 SUP-NATINST는 1,616개의 NLP 작업과 해당 자연어 명령의 대규모 벤치마크임.     
* 55개 언어에 걸친 76개의 광범위한 작업 유형과 같은 다양한 task를 제공        
* 각 task는 **입력 텍스트를 task 출력에 매핑하기 위한 task 정의**와 원하는 또는 **원하지 않는 출력을 보여주기 위한 몇 가지 예제로 구성된 명령**과 쌍을 이룸 (Figure 1)       
* 이러한 task와 instructions 88명의 NLP 실무자가 공개 요청에 따라 제공합니다.     
이러한 기여는 품질을 보장하기 위해 여러 차례의 동료 검토 및 크라우드소싱 피드백을 거친 후 통합됨.      
* 이렇게 다양하고 대규모의 데이터를 보유하면 task를 훈련 및 테스트 세트로 신중하게 나누고 최첨단 방법이 이들에게 어떻게 수행되는지 체계적으로 연구 가능      
* Table 1과 그림 2는 관련 벤치마크와 비교하여 SUP-NATINST의 속성을 강조하며 벤치마크의 작업 및 교육 유형의 다양성을 강조합니다.    


**Figure 1: An example task from SUP-NATINST**     
<img width="217" alt="image" src="https://github.com/yerimoh/img/assets/76824611/4cd3f109-f796-4981-be06-46b0cef76081">

**Table 1: SUP-NATINST를 field에서 주목할 만한 몇 가지 데이터셋과 비교**.             
<img width="416" alt="image" src="https://github.com/yerimoh/img/assets/76824611/9f885db3-a4a5-41cd-b3b9-91975f09081c">
* 우리는 원본 논문에서 다른 데이터 세트의 작업 수, instructions 및 task types을 얻음     
*  "–" 는 적용할 수 없거나 알 수 없음을 나타냅니다.     
*  task type을 분류하는 표준은 데이터 세트마다 다름(Figure 2 참조)      
*  PROMPTSOURCE는 모든 task에 대한 task type 주석을 제공하지 않으며, 대신 T0 교육을 위해 주석이 달린 13개의 task type만 보고함 

**Figure 2: Compared to other datasets**        
<img width="427" alt="image" src="https://github.com/yerimoh/img/assets/76824611/22d563d8-e4c7-43ff-b88e-4d94542ad7c8">
* 다른 데이터셋에 비해 SUP-NATINST는 더 다양한 task types을 지원함      
* InstructGPT의 task types의 매우 거친(coarse) 분류를 하는 걸로 보임    
* 버블 크기는 log scale에서 각 유형의 태스크 수를 나타냄        



**[해결 2) model: Tk-INSTRUCT]**
* 우리의 모델인 Tk-INSTRUCT는 in-context instructions (task definition 또는 _k-shot_ 예제)이 주어진 task input을 변환하기 위한 generative 모델임.      
* T5 모델(Raffel et al., 2020)의 multi-task training에 의해 training 세트의 모든 작업 지침에 걸쳐 구축되며,    
test set에서 unseen task에 대해 평가됨       
* 흥미롭게도, 11B 매개 변수 Tk-INSTRUCT는 119개의 unseen English tasks에서 평가될 때,     
**175B-parameter InstructGPT 모델을 9.9 ROUGE-L points만큼 능가**할 수 있으며,         
multilingual variant mTk-INSTRUCT는 35개의 영어가 아닌 작업에서 **13.3 points만큼 InstructGPT를 능가**함      
* human evaluation에 따르면, Tk-INSTRUCT는 testing instances의 77%(§6.2)에 대한 실제와 맞는 생성하여 **unseen tasks에 대한 강력한 일반화**를 확인한다.        



**[understand the important factors for this generalization]**        
* TkINSTRUCT의 강력한 empirical 성능은 generalizable NLP 모델에 대한 연구를 용이하게 하기 위해 **SUP-NATINST와 같은 초대형 메타 데이터 세트의 중요성을 확인**함      
* 우리는 이 generalization의 중요한 요소를 이해하기 위해 광범위한 분석을 수행함(§7)      
* 우리의 분석은 <span style="background-color:#fff5b1">**training task의 다양성**과 **model size를 확장**하는 것이 **unseen tasks에 대한 강력한 generalization에 중요**</span>하다는 것을 보여줌     
* 마지막으로 성능 상한을 추정하여 추가 개선의 여지를 제시함      


------
-----

# 2 SUPER-NATURALINSTRUCTIONS
   

**[Instruction schema]**      
* 모든 task instruction은 다음 부분으로 구성된  uniform schema (see Fig. 1)를 따른다.         
   * **DEFINITION**     
   주어진 task를 자연어로 정의함    
   이것은 입력 텍스트(예: 문장 또는 문서)가 출력 텍스트에 매핑되는 방식에 대한  complete definition임   
   * **POSITIVE EXAMPLES**      
   긍정적인 예는 각각의 간단한 설명과 함께 입력 및 올바른 출력의 샘플임          
   * **NEGATIVE EXAMPLES**        
   각각의 간단한 설명과 함께 입력 샘플과 incorrect/invalid 출력이 있음     

---

**[Task instances]**     
* 각 task에 대한 instructions이 주어지면 모델이 해당 task의 instances를 해결할 것으로 예상됨     
* 통합된 형식을 사용하여 모든 task의 instances를 구성함       
* 각 instances textual input과 f acceptable textual outputs 목록으로 구성됨     
* 작업 간 인스턴스의 불균형을 방지하기 위해 각 작업의 인스턴스 수를 6.5K로 제한함          

---

**[Benchmark collection]**   
* 벤치마크는 GitHub.3에 대한 대규모 커뮤니티 노력을 통해 수집됨    
* task는 공개 초대에 응답한 NLP 실무자들에 의해 수집 및 기여됨      
  * (a) 기존의 공개 NLP 데이터 세트      
  * (b) 크라우드소싱 실험에서 사용 가능한 중간 주석 (예: QA 데이터 세트를 크라우드소싱하는 동안 질문을 바꾸어 표현하거나 품질을 평가함)      
  * (c) 문장으로 인간에게 전달될 수 있는 synthetic tasks (예: 숫자 비교, 가장 긴 팔인드롬 하위 문자열 찾기, 등)    

---

**[Quality control]**    
* 이 community-contributed 데이터의 품질 관리는 여러 단계로 수행됨.           
* 단계    
**(1)** 제안된 작업의 GitHub 풀 요청을 생성한 후 즉시 automatic test를 거침       
이 프로세스는 도입된 파일에 예상 필드가 포함되어 있고 원하는 속성(예: 중복 인스턴스 없음, 출력 레이블이 크게 불균형하지 않음 등)을 준수하는지 확인함    
**(2)** 제안된 작업은 1-2명의 다른 전문가 기여자에 의해 동료 검토되어 교육 내용의 명확성과 충분성을 보장      
**(3)** 마지막으로, 오타, 명확성 또는 기타 문제(자세한 내용은 §A)와 같은 제공된 지침의 품질에 대한 피드백을 수집하기 위해 추가된 작업을 크라우드 워커에게 제시

---

**[Diversity of tasks]**     
* SUPNATINST를 위한 작업 수집은 다양한 자연어 이해 작업, 도메인 및 언어를 포함하도록 세심하게 수집됨     
* 이러한 다양성을 더 잘 이해하기 위해 task를 세 가지 차원으로 포괄적으로 분류함:     
   * **TASK TYPE**    
   인스턴스 입력에서 출력(예: 질문 답변, 분류 등)에 이르는 매핑의 특성     
   * **LANGUAGE**      
   instances의 언어를 나타냄
   * **DOMAIN**        
   작업 텍스트가 속하는 도메인(예: 정치, 의학, 대화 등)을 나타냄    

이러한 서로 다른 categorization 측도를 사용하여 **일반화의 다양한 의미를 연구** 가능    

----

**[Statistics]**        
* Table 2는 벤치마크에 대한 다양한 통계를 보여줌.        
* 데이터 세트에는 총 1616개의 task과 5M개의 instances가 포함됨        
<img width="221" alt="image" src="https://github.com/yerimoh/img/assets/76824611/a95c3055-6319-4330-b8f4-38237613d277">


----
----

# 3. Tk-INSTRUCT: Learning to Follow Instructions at Scale

Defining Generalization to Unseen Tasks


. Each
task t is defined via its natural language instruction
It
, and each task has a set of input/output instances
(Xt
, Yt). A model M is expected to produce the
output y, given the input x and the task instruction
It
: M(It
, x) = y, for (x, y) ∈ (Xt
, Yt). In particular, we would like to evaluate model M on tasks
that are not observed (i.e., their instances were not
used for training M). The only source of signal
for learning the task at inference time is in-context
instructions It
that contain a definition and demonstration examples of the task.


Tk-INSTRUCT

We introduce Tk-INSTRUCT, a
model that is meta-trained on SUP-NATINST for
solving tasks given their in-context instructions.
Previous work has shown the effectiveness of such
meta-training in improving model’s ability to do incontext learning with either prompts (Zhong et al.,
2021; Sanh et al., 2022) or demonstration examples
(Min et al., 2022a). Because of the large variety
of tasks in SUP-NATINST, we are able to do this
multi-task meta-training at a larger scale than before. We conduct our experiments and analysis
based on the T5 model (Raffel et al., 2020). Since
each instruction It consists of multiple elements as
described in our instruction schema (§3), we map
these elements to textual format and append them
before the input instance. Fig. 8 in the appendix
shows how we encode the full instructions. We
study different combinations of these instruction
elements in §7.2. By default, we will use our most
effective instruction elements (i.e., task definition
and two positive examples) unless otherwise specified. In the same manner, we train the multilingual
variant mTk-INSTRUCT based on the mT5 model
(Xue et al., 2021).


