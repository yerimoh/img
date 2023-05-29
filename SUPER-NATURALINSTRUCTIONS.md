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
SUPER-NATURALINSTRUCTIONS is a metadataset (Triantafillou et al., 2019) consisting of a variety of NLP tasks (see Fig. 2a) and instructions that describe them in plain language. Instruction schema. All task instructions follow the same uniform schema (see Fig. 1) which is composed of the following parts: • DEFINITION defines a given task in natural language. This is a complete definition of how an input text (e.g., a sentence or a document) is expected to be mapped to an output text.

• POSITIVE EXAMPLES are samples of inputs and their correct outputs, along with a short explanation for each.

• NEGATIVE EXAMPLES are samples of inputs and their incorrect/invalid outputs, along with a short explanation for each.

The above schema is based on that of Mishra et al. (2022b), though it is simplified. See Appendix C for the comparison.

Task instances.
Given the instructions for each task, a model is expected to solve instances of that task. We use a unified format to organize the instances of all our tasks. More precisely, each instance consists of a textual input and a list of acceptable textual outputs. We limit the number of instances in each task to 6.5K to avoid an imbalance of instances between tasks

SUPER-NURATURE INstructions는 다양한 NLP 작업(그림 2a 참조)과 이를 평이한 언어로 설명하는 지침으로 구성된 메타데이터 세트(Triantafillou et al., 2019)입니다. 명령 스키마. 모든 작업 지침은 다음 부분으로 구성된 동일한 스키마(그림 1 참조)를 따릅니다. • DEFINITION은 주어진 작업을 자연어로 정의합니다. 이것은 입력 텍스트(예: 문장 또는 문서)가 출력 텍스트에 매핑되는 방식에 대한 완전한 정의입니다.

• 긍정적인 예는 각각의 간단한 설명과 함께 입력 및 올바른 출력의 샘플입니다.

• 부정적인 예로는 각각의 간단한 설명과 함께 입력 샘플과 잘못된/잘못된 출력이 있습니다.

상기 스키마는 Mishra et al. (2022b)의 스키마를 기반으로 하지만 단순화됩니다. 비교는 부록 C를 참조하십시오.

태스크 인스턴스.
각 작업에 대한 지침이 주어지면 모델이 해당 작업의 인스턴스를 해결할 것으로 예상됩니다. 통합된 형식을 사용하여 모든 작업의 인스턴스를 구성합니다. 보다 정확하게는 각 인스턴스는 텍스트 입력과 허용 가능한 텍스트 출력 목록으로 구성됩니다. 작업 간 인스턴스의 불균형을 방지하기 위해 각 작업의 인스턴스 수를 6.5K로 제한합니다

벤치마크 컬렉션.
벤치마크는 GitHub.3에 대한 대규모 커뮤니티 노력을 통해 수집되었습니다 과제는 공개 초대에 응답한 NLP 실무자들에 의해 수집 및 기여되었습니다.4 또는 학급 프로젝트의 일부로 기여하도록 권장된 학생들.5 기여자들은 창의적이고 여러 리소스에서 과제를 조달하도록 권장되었습니다. (a) 기존의 공개 NLP 데이터 세트, (b) 크라우드소싱 실험에서 사용 가능한 중간 주석(예: QA 데이터 세트를 크라우드소싱하는 동안 질문을 바꾸어 표현하거나 품질을 평가함) 또는 (c) 몇 문장으로 평균적인 인간에게 전달될 수 있는 합성 작업(예: 숫자 비교, 가장 긴 팔인드롬 하위 문자열 찾기, 등). 기존 데이터 세트 또는 크라우드소싱 주석을 사용할 때, 기여자는 가능할 때마다 이 데이터 세트를 만드는 데 사용되는 지침을 채택하도록 권장되었습니다. 이 작업은 지침이 평균적인 인간 판독기에 대한 작업을 정의하기에 충분하도록 보장하기 위해 수행되었습니다. 작업은 지침 및 기타 메타 정보와 함께 GitHub 풀 요청을 통해 JSON 파일로 제공되었으며 자동 검사 및 피어에서 이를 검토했습니다. 다양한 장소와 배경의 88명의 기여자가 저장소에 기여했습니다

품질 관리.
이 커뮤니티에서 제공하는 데이터의 품질 관리는 여러 단계로 수행되었습니다. (1) 제안된 작업의 GitHub 풀 요청을 생성한 후 즉시 자동 테스트를 거쳤습니다. 이 프로세스는 도입된 파일에 예상 필드가 포함되어 있고 원하는 속성(예: 중복 인스턴스 없음, 출력 레이블이 크게 불균형하지 않음 등)을 준수하는지 확인했으며, (2) 제안된 작업은 1-2명의 다른 전문가 기여자에 의해 동료 검토되어 교육 내용의 명확성과 충분성을 보장했습니다. 검토 프로세스는 검토자들이 제안된 지침의 품질에 만족할 때까지 반복적으로 수행되었습니다. 특히, 검토자들은 일반적인 언어 화자가 문법적이고 유창하며 간결하면서 기본 작업(평가 인스턴스)을 해결하기에 충분하고 명확한 지침인지 확인하도록 요청받았습니다. 각 GitHub 풀 요청에 대한 검토는 통합되기까지 며칠 동안 평균적으로 약 4-6회 반복되었습니다. (3) 마지막으로, 오타, 명확성 또는 기타 문제(자세한 내용은 §A)와 같은 제공된 지침의 품질에 대한 피드백을 수집하기 위해 추가된 작업을 크라우드 워커에게 제시했습니다. 그 후, 작성자 중 한 명이 이 피드백을 사용하여 인스턴스의 작업 정의를 개선했습니다. 이 피드백은 다른 언어로 고품질 크라우드 워커를 찾는 것이 중요하지 않기 때문에 영어 작업에만 수행되었습니다(Pavlick et al., 2014)

작업의 다양성.
SUPNATINST를 위한 작업 수집은 다양한 자연어 이해 작업, 도메인 및 언어를 포함하도록 세심하게 감독되었습니다. 이러한 다양성을 더 잘 이해하기 위해 작업을 세 가지 차원으로 포괄적으로 분류합니다:

• 작업 유형은 인스턴스 입력에서 출력(예: 질문 답변, 분류 등)에 이르는 매핑의 특성을 정의합니다.

• 언어는 인스턴스의 언어를 나타냅니다.

• 도메인은 작업 텍스트가 속하는 도메인(예: 정치, 의학, 대화 등)을 나타냅니다.

이러한 서로 다른 범주화 측도를 사용하여 일반화의 다양한 의미를 연구할 수 있습니다. 경험적 연구(§5)에서, 우리는 작업 유형의 축을 따라 일반화를 연구합니다. 다양한 작업 유형, 언어 및 도메인 간의 작업 분배를 위해 부록의 그림 10을 참조하십시오.

Benchmark collection.
The benchmark was collected through a large community effort on GitHub.3 Tasks were collected and contributed by NLP practitioners who were either responding to our public invitation4 or students who were encouraged to contribute as part of their class project.5 Contributors were encouraged to be creative and source the tasks from several resources: (a) existing public NLP datasets, (b) available intermediate annotations in crowdsourcing experiments (e.g., paraphrasing questions or rating their quality during crowdsourcing a QA dataset), or (c) synthetic tasks that can be communicated to an average human in a few sentences (e.g., basic algebraic operations like number comparison, finding the longest palindrome substring, etc.). When using existing datasets or crowdsourcing annotations, contributors were encouraged to adopt the instructions used to create this dataset whenever available. This was done to ensure that the instructions were sufficient to define the tasks to average human readers. Tasks along with instructions and other meta information were contributed as JSON files via GitHub pull requests, which were reviewed by automated checks and peers. We had 88 contributors from diverse locations and backgrounds contribute to our repository

Quality control.
Controlling the quality of this community-contributed data was done in several phases: (1) Upon creating a GitHub pull request of the proposed task, it immediately went through an automatic test. This process verified that the introduced file contained the expected fields and adhered to our desired properties (e.g., no duplicate instances, the output labels are not heavily imbalanced, etc.) and (2) The proposed task was then peer-reviewed by 1–2 other expert contributors to ensure the clarity and sufficiency of instruction content. The review process was done iteratively until the reviewers were content with the quality of the proposed instruction. Specifically, reviewers were asked to verify that the instruction is clear and sufficient for an average language speaker to solve the underlying task (evaluation instances) while being grammatical, fluent, and concise. On average, the review of each GitHub pull request took about 4– 6 iterations over the span of multiple days before being merged. (3) Lastly, the added tasks were presented to crowdworkers in order to collect feedback on the quality of the provided instructions, such as typos, clarity, or other issues (details in §A). Subsequently, one of the authors used this feedback to improve the task definitions of the instances. This feedback was done only for English tasks, as finding high-quality crowdworkers in other languages is nontrivial (Pavlick et al., 2014)

Diversity of tasks.
Collecting tasks for SUPNATINST was carefully supervised to cover a wide variety of natural language understanding tasks, domains, and languages. To better understand this diversity, we comprehensively categorize tasks along three different dimensions:

• TASK TYPE defines the nature of the mapping from instance inputs to outputs (e.g., question answering, classification, etc.).

• LANGUAGE indicates the language(s) of the instances.

• DOMAIN indicates the domain(s) to which the text of the tasks belong to (e.g., politics, medicine, dialogue, etc.).

These different measures of categorization can be used to study different senses of generalization. In our empirical studies (§5), we study generalization along the axis of task types. We refer the reader to Fig. 10 in the appendix for the distribution of tasks among different task types, languages, and domains.



Statistics.    
Table 2 shows various statistics for the
benchmark. In total, the dataset includes 1616 tasks
and 5M instances. On average, each instruction is
paired with 2.8 positive and 2.4 negative examples.
The average definition length is 56.6 in words.













