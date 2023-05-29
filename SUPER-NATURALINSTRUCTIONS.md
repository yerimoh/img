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
The NLP community has witnessed great progress
 in building models for generalization to unseen
 tasks via in-context instructions using
 large pretrained language models (Raffel et al.,
 2020; Brown et al., 2020). As remarkable as mod
els like InstructGPT (Ouyang et al., 2022) are, the
 contribution of various design choices to their suc
cess is opaque. In particular, the role of super
vised data has remained understudied due to lim
ited data released by the corporate entities behind
 major models. In addition, it is nearly impossible
 for the research community to extend and re-train
 these gigantic models. Addressing these two cha
 lenges necessitates the availability of large-scale
public benchmarks of a broad range of NLP tasks
and their instructions to facilitate developing and
evaluating models that can generalize to unseen
tasks
 
 
 In this paper, we construct a meta-dataset (i.e.,
dataset of datasets; Triantafillou et al., 2019) that
consists of a wide variety of NLP tasks with their
instructions, and train a model that can perform
a new task given the instruction, outperforming
InstructGPT (which uses 16× more parameters).

Our dataset, SUPER-NATURALINSTRUCTIONS
(SUP-NATINST for short), is a large benchmark of
1,616 NLP tasks and their natural language instructions. It brings in a diverse variety of tasks—76
broad task types spanning 55 different languages.
Each task is paired up with an instruction that consists of the task definition for mapping an input text
to a task output and several examples for demon
strating the desired or undesired output (see Fig.1
as an example task). These tasks and their instructions are contributed by 88 NLP practitioners, in
response to our public call. These contributions are
consolidated after several rounds of peer-review
and crowdsourced feedback to ensure quality. Having this diverse and large-scale data enables us
to carefully split the tasks into training and test
sets and systematically study how state-of-the-art
methods perform on them. Table 1 and Figure 2
highlight properties of SUP-NATINST compared to
relevant benchmarks, emphasizing the diversity of
tasks and instruction types in our benchmark.


Our model, Tk-INSTRUCT, is a generative
model for transforming task inputs given declarative in-context instructions (task definition or kshot examples). It is built by multi-task training
of the T5 model (Raffel et al., 2020) over all the
task instructions in our training set, and is eval
uated on unseen tasks in the test set. Interestingly, an 11B-parameter Tk-INSTRUCT can outperform the 175B-parameter InstructGPT model
by 9.9 ROUGE-L points when evaluated on 119
unseen English tasks, and the multilingual variant
mTk-INSTRUCT outperforms InstructGPT by 13.3
points on 35 non-English tasks (§6.1). According
to human evaluation, Tk-INSTRUCT generates responses at least as well as the ground truth for 77%
of the testing instances (§6.2), confirming its strong
generalization to unseen tasks.


The compelling empirical performance of TkINSTRUCT confirms the importance of super-sized
meta datasets such as our SUP-NATINST to facilitate research towards generalizable NLP models.
We conduct extensive analysis to understand the
important factors for this generalization (§7). Our
analysis shows that scaling up the diversity of training tasks and the model size are both important
for strong generalization to unseen tasks. Finally,
we estimate performance upper bounds, suggesting
further room for improvement.


 
 
 
 
 
