# SUPER-NATURALINSTRUCTIONS:  Generalization via Declarative Instructions on 1600+ NLP Tasks


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


<img width="296" alt="image" src="https://github.com/yerimoh/img/assets/76824611/24505140-e539-4f51-b192-fe057cc057eb">
Evaluation categories, their evaluation metrics   

각 task number에 따른 예
<img width="338" alt="image" src="https://github.com/yerimoh/img/assets/76824611/6d61c539-e808-4441-87dc-e6dc8c27f46c">





<details>
<summary>📜 예시 더보기</summary>
<div markdown="1">
  
 <img width="339" alt="image" src="https://github.com/yerimoh/img/assets/76824611/b7212697-9a1d-48d3-b4fb-f2158336c7af">
<img width="335" alt="image" src="https://github.com/yerimoh/img/assets/76824611/3e068a3f-6cd0-431e-940e-3bb861e4d2ab">
<img width="337" alt="image" src="https://github.com/yerimoh/img/assets/76824611/b3f56d08-812c-4be8-b1b8-7682dfb43607">
<img width="332" alt="image" src="https://github.com/yerimoh/img/assets/76824611/1b3e9fc8-f3e4-47fe-99d4-81b701ec967d">
<img width="345" alt="image" src="https://github.com/yerimoh/img/assets/76824611/be5cb3e0-055a-46d1-9ab6-c8e5a3de90a9">
<img width="342" alt="image" src="https://github.com/yerimoh/img/assets/76824611/246f0675-944c-43ae-aff3-2d379c576bb8">
<img width="333" alt="image" src="https://github.com/yerimoh/img/assets/76824611/e2606815-404c-4cd0-9b3b-4fd5773dad63">
<img width="334" alt="image" src="https://github.com/yerimoh/img/assets/76824611/0660a175-2372-4727-a52a-637223d0b4ef">
<img width="325" alt="image" src="https://github.com/yerimoh/img/assets/76824611/70af7dda-c9b4-4990-993f-ddc3267135ee">
<img width="340" alt="image" src="https://github.com/yerimoh/img/assets/76824611/044be002-7274-4ffc-96bb-ab7318e49799">
<img width="346" alt="image" src="https://github.com/yerimoh/img/assets/76824611/9be42642-c3f0-4c4a-acd1-d06b19110690">


 
</div>
</details>  


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

**[Defining Generalization to Unseen Tasks]**          
* 각 task $$t$$는 natural language instruction $$I_t$$을 통해 정의됨       
* 각 task에는 일련의 input/output instances($$X_t$$, $$Y_t$$)가 존재     
* **목표:**    
입력 $$x$$와 task instruction $$I_t: M(I_t, x) = y(x, y) ∈(X_t, Y_t)$$에 대해 $$M$$ 모델이 출력 $$y$$를 생성해야 함.        
* **평가:**    
특히 not observed(즉, 해당 instances가 M 훈련에 사용되지 않음)에 대해 모델 $$M$$을 평가하고자 함     
* inference 시간에 task 학습하기 위한 유일한 source of signal는 작업의 definition 및 d demonstration examples를 포함하는 컨텍스트 내 instructions($$I_t$$)임     

----

**[Tk-INSTRUCT]**      
* 우리는 <span style="color:#ffd33d">**SUP-NATINST에서 meta-trained을 받은 모델**인 Tk-INSTRUCT를 소개</span>함     
* 이 모델은 **context 내 instruction에 따라 task를 해결함**       
* SUP-NATINST의 <span style="color:#ffd33d">**다양한 task**로 인해 **이전보다 더 큰 규모로 이 multi-task meta-training을 수행 가능**</span>       
* [T5 모델](https://yerimoh.github.io/LAN24/)을 기반으로 실험 및 분석을 수행      
* 각 명령어는 (§2)에 설명된 대로 여러 요소로 구성되므로, 이러한 요소를 텍스트 형식에 매핑하고 입력 인스턴스 앞에 추가함.     
* 아래 전체 instruction을 인코딩하는 방법을 보여줌     
<img width="205" alt="image" src="https://github.com/yerimoh/img/assets/76824611/d77e9c2d-9de5-441a-a4a0-35bb5aba52fd">
* 우리는 (§ 6.2 Instructing with Different Elements)에서 이러한 instruction들의 다양한 조합을 연구함       
* 달리 명시되지 않는 한, 기본적으로 가장 효과적인 instruction 요소(즉, task definition와 두 가지 positive example)를 사용      
----
----


# 4 Benchmarking Cross-Task
Generalization with SUP-NATINST Here we provide our recommended recipe for benchmarking generalization via SUP-NATINST.

SUP-NATINST를 통한 일반화 SUP-NATINST를 통한 일반화 벤치마크를 위한 권장 레시피를 제공합니다.

## 5.1 Evaluation Setup
**[An Evaluation Split of Unseen Tasks]**      
* SUP-NATINST의 대규모 task 모음을 **two subsets으로 나눔**:    
  * **evaluation용 task**   
  154개 작업을 나타내는 12개 범주의 manuallyselected 컬렉션을 fix   
  * **supervision용 task**            


    
<details>
<summary>📜 대표적인 task에 대한 예제와 함께 evaluation task 보</summary>
<div markdown="1">

<img width="307" alt="image" src="https://github.com/yerimoh/img/assets/76824611/db62316a-6170-482b-8859-812d8d4a73a0">
 
 </div>
</details>  




**[Divided Tracks for English and X-lignual Tasks]**      
* SUP-NATINST는 여러 언어에 걸친 task로 구성되어 영어뿐만 아니라 다른 언어로도 unseen tasks에 대한 모델의 generalization을 평가 가능     
* 따라서 evaluation tasks을 two tracks으로 나눔     
  * **English cross-task generalization (119 tasks)**     
  * **cross-lingual cross-task generalization (35 tasks)**      
  즉, 다른 언어로 unseen task에 대한 일반화          
  



**[Evaluation Metrics]**     
* ROUGE-L(Lin, 2004)을 채택      
이는 광범위한 텍스트 생성 작업에 적용할 수 있는 soft string overlap metric니다.     
* human evaluation도 사용

----
---

# 5. Experimental Results   

<img width="288" alt="image" src="https://github.com/yerimoh/img/assets/76824611/0d5f329b-f356-46de-972c-4027e38695e6">
* Instruction-tuning enables **stronger generalization** to unseen tasks      
* Our **Tk-INSTRUCT outperforms** InstructGPT      
* There is a **sizable gap for improvement**     


----
----


# 6. Conclusion
* 다양한 NLP taskset와 그 instructions으로 구성된 large-scale benchmark를 구성함.        
* 이 benchmark는 **instructions 따름으로써 unseen tasks로 generalize**할 수 있는 모델의 train 또는 evaluation를 위한 풍부한 역할을 할 수 있음.        
* 또한 **이 데이터를 사용하여 Tk-INSTRUCT를 train**하고 unseen tasks를 놀라운 수준으로 수행하는 능력을 보여줌         
* 우리는 그러한 generalization의 중요한 요소를 이해하기 위해 광범위한 분석을 제공함      
* 리는 우리의 데이터와 모델이 더 범용적인 모델을 향한 향후 작업을 촉진하기를 바람       

----
----

# 7. Limitations

**[distributions의 문제]**            
* 제시된 데이터는 variety(예: 다양한 ask types) 제공하지만, distributions는 향후 작업에서 다루어야 하는 왜곡으로 인해 어려움을 겪고 있음(Evaluation Metrics의 평가 정단성 문제)       
* language diversity에 대해 제안된 벤치마크는 영어에 편향되어 있음       
* output diversity에서 수집된 작업은 일반적으로 여전히 짧은 응답으로 치우쳐 있으며, 이는 현장에서 사용 가능한 tasks의 distribution를 반영할 수 있음        
➡ 긴 tasks의 이러한 **과소 표현은 미래에 범용 모델을 구축하는 데 어려움을 제기**함.        
* 우리는 앞으로의 작업이 그러한 distribution 불균형을 해결하기를 바람.        
* 게다가, 우리는 비전이나 연설과 같은 다른 양식의 맥락에서 여기 설정된 이후의 명령어의 자연스러운 확장을 봅니다.


**[Automatic evaluation of models’ performance]**      
* benchmark의 다양한 task 세트와 그 중 많은 taks가 **open-ended generation tasks**임을 고려할 때 **모델의 성능을 자동으로 평가하는 것은 또 다른 과제**임.       
* 우리는 본 논문에서 ROUGE-L을 aggregated metric으로 사용하고 인간의 평가와 잘 일치하여 모델의 전반적인 성능에 대한 좋은 프록시로 찾음          
* 그러나 ROUGE-L이 품질의 효과적인 프록시 역할을 하지 못할 수 있는 특정 작업이 존재함          
(eg: 입력을 복사하면 높은 ROUGE-L 점수를 얻을 수 있는 rewriting tasks 또는 error correction tasks)             
* 텍스트 생성을 위한 보다 **강력한 평가 지표 개발로 이러한 문제가 해결되기를 바람**      



**[예산의 한계]**       
* 컴퓨팅 성능 측면에서, 우리는 우리가 접근할 수 있는 모델을 실험했고 결과 모델을 공개적으로 사용할 수 있게 함        
* 우리는 또한 computational 예산의 한계로 인해 train할 수 없었던 더 큰 모델이 있다는 것을 인정함     


