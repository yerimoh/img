# UPER-NATURALINSTRUCTIONS:  Generalization via Declarative Instructions on 1600+ NLP Tasks


# Abstract
 How well can NLP models generalize to a va
riety of unseen tasks when provided with task
 instructions? To address this question, we first
 introduce SUPER-NATURALINSTRUCTIONS,1
 a benchmark of 1,616 diverse NLP tasks and
 their expert-written instructions. Our collec
tion covers 76 distinct task types, including but
 not limited to classification, extraction, infill
ing, sequence tagging, text rewriting, and text
 composition. This large and diverse collec
tion of tasks enables rigorous benchmarking of
 cross-task generalization under instructions—
 training models to follow instructions on a sub
set of tasks and evaluating them on the remain
ing unseen ones.
 Furthermore, we build Tk-INSTRUCT, a trans
former model trained to follow a variety of in
context instructions (plain language task defi
nitions or k-shot examples). Our experiments
 show that Tk-INSTRUCT outperforms existing
 instruction-following models such as Instruct
GPTbyover9%onourbenchmark despite be
ing an order of magnitude smaller. We further
 analyze generalization as a function of various
 scaling parameters, such as the number of ob
served tasks, the number of instances per task,
 and model sizes. We hope our dataset and
 model facilitate future progress towards more
 general-purpose NLP models.2
 
 
 
 # 1 Introduction
 The NLP community has witnessed great progress
 in building models for generalization to unseen
 tasks via in-context instructions (Mishra et al.,
 2022b; Sanh et al., 2022; Wei et al., 2022) using
 large pretrained language models (Raffel et al.,
 2020; Brown et al., 2020). As remarkable as mod
els like InstructGPT (Ouyang et al., 2022) are, the
 contribution of various design choices to their suc
cess is opaque. In particular, the role of super
vised data has remained understudied due to lim
ited data released by the corporate entities behind
 major models. In addition, it is nearly impossible
 for the research community to extend and re-train
 these gigantic models. Addressing these two chal
 lengesnecessitatestheavailabilityoflarge-scale
 publicbenchmarksofabroadrangeofNLPtasks
 andtheirinstructionstofacilitatedevelopingand
 evaluatingmodelsthatcangeneralizetounseen
 tasks.
 
 
 Inthispaper,weconstructameta-dataset(i.e.,
 datasetofdatasets;Triantafillouetal.,2019)that
 consistsofawidevarietyofNLPtaskswiththeir
 instructions,andtrainamodelthatcanperform
 anewtaskgiventheinstruction,outperforming
 InstructGPT(whichuses16moreparameters).
 
 
 
 Ourdataset,SUPER-NATURALINSTRUCTIONS
 (SUP-NATINSTforshort),isalargebenchmarkof
 1,616NLPtasksandtheirnaturallanguageinstruc
tions.Itbringsinadiversevarietyoftasks—76
 broadtasktypesspanning55differentlanguages.
 Eachtaskispairedupwithaninstructionthatcon
sistsofthetaskdefinitionformappinganinputtext
 toataskoutputandseveralexamplesfordemon
stratingthedesiredorundesiredoutput(seeFig.1
 asanexampletask).Thesetasksandtheirinstruc
tionsarecontributedby88NLPpractitioners,in
 responsetoourpubliccall.Thesecontributionsare
 consolidatedafterseveralroundsofpeer-review
 andcrowdsourcedfeedbacktoensurequality.Hav
ingthisdiverseandlarge-scaledataenablesus
 tocarefullysplitthetasksintotrainingandtest
 setsandsystematicallystudyhowstate-of-the-art
 methodsperformonthem.Table1andFigure2
 highlightpropertiesofSUP-NATINSTcomparedto
 relevantbenchmarks,emphasizingthediversityof
 tasksandinstructiontypesinourbenchmark
 
 
 Ourmodel,Tk-INSTRUCT,isagenerative
 modelfortransformingtaskinputsgivendeclar
ativein-contextinstructions(taskdefinitionork
shotexamples).Itisbuiltbymulti-tasktrainin
 of the T5 model (Raffel et al., 2020) over all the
 task instructions in our training set, and is eval
uated on unseen tasks in the test set. Interest
ingly, an 11B-parameter Tk-INSTRUCT can out
perform the 175B-parameter InstructGPT model
 by 9.9 ROUGE-L points when evaluated on 119
 unseen English tasks, and the multilingual variant
 mTk-INSTRUCT outperforms InstructGPT by 13.3
 points on 35 non-English tasks (§6.1). According
 to human evaluation, Tk-INSTRUCT generates re
sponses at least as well as the ground truth for 77%
 of the testing instances (§6.2), confirming its strong
 generalization to unseen tasks.
 
 
 The compelling empirical performance of Tk
INSTRUCT confirms the importance of super-sized
 meta datasets such as our SUP-NATINST to facil
itate research towards generalizable NLP models.
 We conduct extensive analysis to understand the
 important factors for this generalization (§7). Our
 analysis shows that scaling up the diversity of train
ing tasks and the model size are both important
 for strong generalization to unseen tasks. Finally,
 we estimate performance upper bounds, suggesting
 further room for improvement.
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
