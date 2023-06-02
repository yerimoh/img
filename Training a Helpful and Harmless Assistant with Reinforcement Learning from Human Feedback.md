# Training a Helpful and Harmless Assistant with Reinforcement Learning from Human Feedback


# Abstract


We apply preference modeling and reinforcement learning from human feedback (RLHF) to finetune language models to act as helpful and harmless assistants

alignment training가 거의 모든 NLP 평가에서 성능을 향상시키고 파이썬 코딩 및 요약과 같은 전문 기술 훈련과 완전히 호환

preference modeling과 RL 정책이 매주 새로운 인간 피드백 데이터로 업데이트되어 데이터 세트와 모델을 효율적으로 개선하는 반복적인 online mode of training를 탐구합니다        


우리는 RLHF 훈련의 견고성을 조사하고, RL policy 정책과 initialization 사이의 KL 발산의 제곱근 사이의 대략적인 선형 관계를 식별합니다. 


 주요 결과와 함께, 우리는 교정, 경쟁 목표 및 OOD 탐지 사용(calibration, competing objectives, and the use of OOD detection, compare our models with human writers)에 대한 주변 분석을 수행하고, 우리의 모델을 인간 작가와 비교하고, 최근 관련 작업에 나타나는 프롬프트를 사용하여 모델의 샘플을 제공합니다.
 
 
우리의 목표는 '도움이 되는' 것과 '무해한' 것이 무엇을 의미하는지 정의하거나 규정하는 것이 아니라 훈련 기술의 효과를 평가하는 것이기 때문에 대부분의 경우 크라우드 워커가 이러한 개념을 적합하다고 생각하는 대로 해석하도록 합니다. 우리는 유용성과 무해성을 개별적으로 처리하여 각각의 뚜렷한 인간 선호 데이터 세트를 수집합니다. 도움을 위해 크라우드 워커에게 질문에 답하거나 문서를 작성 또는 편집하거나 계획 및 결정에 대한 논의와 같은 순수한 텍스트 기반 작업을 지원하기 위해 모델을 요청합니다. 


무해함을 위해, 우리는 크라우드 워커들을 적대적으로 조사하거나 우리의 언어 모델을 '레드 팀'으로 초대하여 해로운 반응을 유발합니다: 은행 강도 계획과 같은 해로운 목표를 도와주거나 AI가 유독한 언어를 사용하도록 하기 위해서입니다.2 AI 비서와 대화하는 각 단계에서 크라우드 워커는 두 가지 가능한 응답을 제공합니다. 도움 작업에 참여하는 사람들은 보다 도움이 되고 정직한(즉, 더 나은) 응답을 선택하라는 지침을 받습니다. 빨간색 팀 구성 작업에 참여하는 사람들은 더 유해한(즉, 더 나쁜) 반응을 선택하라는 지시를 받습니다. 이러한 대화와 표현된 인간 선호도는 데이터 세트를 형성합니다.3

 
 우리의 목표는 '도움이 되는' 것과 '무해한' 것이 무엇을 의미하는지 정의하거나 규정하는 것이 아니라 훈련 기술의 효과를 평가하는 것이기 때문에 대부분의 경우 크라우드 워커가 이러한 개념을 적합하다고 생각하는 대로 해석하도록 합니다. 우리는 유용성과 무해성을 개별적으로 처리하여 각각의 뚜렷한 인간 선호 데이터 세트를 수집합니다. 도움을 위해 크라우드 워커에게 질문에 답하거나 문서를 작성 또는 편집하거나 계획 및 결정에 대한 논의와 같은 순수한 텍스트 기반 작업을 지원하기 위해 모델을 요청합니다. 무해함을 위해, 우리는 크라우드 워커들을 적대적으로 조사하거나 우리의 언어 모델을 '레드 팀'으로 초대하여 해로운 반응을 유발합니다: 은행 강도 계획과 같은 해로운 목표를 도와주거나 AI가 유독한 언어를 사용하도록 하기 위해서입니다.2 AI 비서와 대화하는 각 단계에서 크라우드 워커는 두 가지 가능한 응답을 제공합니다. 도움 작업에 참여하는 사람들은 보다 도움이 되고 정직한(즉, 더 나은) 응답을 선택하라는 지침을 받습니다. 빨간색 팀 구성 작업에 참여하는 사람들은 더 유해한(즉, 더 나쁜) 반응을 선택하라는 지시를 받습니다. 이러한 대화와 표현된 인간 선호도는 데이터 세트를 형성합니다.3

도움이 되는 것과 해롭지 않은 것은 종종 서로 반대되는 것입니다. 해를 피하는 데 지나치게 집중하면 실제로 인간의 요구를 해결하지 못하는 '안전한' 대응으로 이어질 수 있습니다. 도움이 되는 것에 대한 지나친 집중은 인간이 해를 끼치거나 독성 콘텐츠를 생성하도록 돕는 반응으로 이어질 수 있습니다. 우리는 이러한 품질 중 하나를 일차적으로 평가하도록 훈련된 선호 모델이 다른 하나에서 매우 낮은 성능(우연성보다 훨씬 나쁜)을 발휘한다는 것을 보여줌으로써 이러한 긴장을 정량적으로 보여줍니다. 

다행스럽게도, 우리는 두 데이터 세트의 혼합에 대해 훈련된 PM이 그럼에도 불구하고 유해한 요청의 정중한 거절을 장려하면서 적절한 교훈을 배우고 적절한 경우 도움이 되는 행동을 할 수 있다는 것을 발견했습니다. 선호 모델을 손에 쥐고, 우리는 PM 점수를 보상으로 사용하여 강화 학습을 통해 도움이 되고 무해한 보조자를 훈련합니다. 우리는 PM 성능과 RLHF 훈련 모델의 보다 관련성 있는 성능 특성을 모두 평가합니다. 그림 1에서 볼 수 있듯이, 순수하게 도움이 되는 RLHF 훈련 모델은 레드 팀에 훨씬 더 쉬운 반면, 도움이 되는 + 무해한 모델은 매우 도움이 되고 훨씬 덜 해롭습니다

정렬 훈련과 관련하여 자주 제기되는 문제는 그것이 AI 기능을 손상시킬 것인지 여부입니다. 우리는 RLHF가 큰 언어 모델에 적용될 때, 대답은 거의 범주적인 no인 것처럼 보인다는 것을 발견했습니다. RLHF 훈련 모델은 그림 3에 요약된 것처럼 거의 모든 평가에서 원시 생성 모델보다 성능이 우수한 경향이 있습니다. 우리는 또한 정렬 또는 성능을 저하시키지 않으면서 전문 기술과 정렬 관련 교육을 혼합할 수 있다고 주장합니다. 실제로 정렬된 모델은 원시 모델보다 사용자 친화적이고 배치 가능성이 높기 때문에 정렬에 미세하게 조정되지 않은 모델을 배치할 이유가 거의 없음을 의미합니다
 
7.1 Limitations   


While we believe our results present a promising picture for the alignment of existing language models,
work on this subject remains in an early stage, and has a number of limitations. As was also emphasized
by the authors of [Thoppilan et al., 2022], we view our work on alignment as an ongoing project; our work
[Askell et al., 2021] was step zero, and this is step one.
We’ve pragmatically defined an aligned assistant as an AI that is18 helpful, honest, and harmless. We are optimistic that at present capability levels, the techniques we have discussed here provide a reasonable approach
to achieving helpfulness and harmlessness. However, although our techniques improve model honesty, we
believe we are just scratching the surface of that problem, and that other techniques may more efficiently and
effectively produce honest AI models.
Here we have essentially focused on the average-case behavior of our models. However, even if we were
convinced that our models were HHH in expectation, a clear next step would be to attempt to study and
eliminate bad behaviors (especially harmfulness) even in the worst case. We have not addressed this question
of robustness here, but hope to study it in the future (approaches such as [Perez et al., 2022] may be useful). It
will only become more pressing as AI systems advance and encounter distributional shift during deployment.
AI alignment may be difficult and ambiguous to assess. So for example, while our large RLHF-trained
models perform better than plain LMs on virtually all capabilities evaluations, one might hope that a truly
helpful models’ zero-shot performance would equal the few-shot performance of an unaligned model. The
logic here is that if a model can really ‘helpfully follow instructions’, then a prompt or explanation should
be sufficient to bridge the zero-to-few-shot gap. We are very far from achieving this level of performance!
Even on the honesty evaluation TruthfulQA [Lin et al., 2021] we close a bit less than half of this gap (Figure
5). We also briefly investigated whether our RLHF-finetuned code models have any comparative advantage
when exposed to prompts including buggy code [Chen et al., 2021], but we did not find any benefits there.
One would hope a fully aligned model would do its best to write correct code, even when given a buggy
prompt.
We also harbor a general concern that perhaps our techniques only render models aligned ‘on the surface’,
and that they still harbor harmful biases or other tendencies that may surface in more subtle contexts. We
found that RLHF models have a more positive sentiment towards all racial and religious groups, which seems
promising, but does not necessarily indicate that biases have been reduced. And with respect to gender, we
found that RLHF model biases are very strongly correlated with the bias of the underlying language models.
That said, further work will be required to understand if this is a limitation of RLHF as a technique, or of
our particular HH datasets. In any case, we likely need to build more subtle and comprehensive evaluations
that include multi-turn dialogue, as this is an area where humans will likely use the models, and it’s also a
place where it’s inherently more difficult to measure performance against subtle objectives such as bias and
fairness.
On a much more practical level, we do not have much experience applying RL techniques to large generative
models. Experienced AI practitioners know that there are a large variety of tweaks and tricks that require
experimentation to identify, and that can majorly improve the stability and performance of training. We have
18To be clear, we mean truly, thoroughly, and fundamentally, and not ‘merely behaviorally’ in some limited contexts.
35
encountered some stability issues with RL, and although we performed some rudimentary hyperparameter
scans, we expect that with more experience and study we could do better. We also did not explore variations
in online training, such as literally updating a single PM or RLHF model; rather we retrained these models
from scratch on each iteration. Another direction for exploration is to use a non-trivial function of PM scores
as the RL reward, distorting the score distribution to e.g. focus more on discouraging bad behavior rather
than rewarding good behavior. In summary, there are many future directions to explore for improving RLHF.
A final concern is whether techniques like those we have employed will continue to apply as AI models
become increasingly capable. We take these concerns very seriously. In our view, the present work makes
some progress towards our initial goal, which is to establish a set of simple and universal techniques19 that
can align AI models at present capability levels. Assuming this goal can be met, one of the next steps will be
to build consensus among researchers and to understand alignment in greater depth, including how techniques
scale with AI capabilities. The hope will be to create an evolving pragmatic state of the art for training AIs
that are thoroughly helpful, honest, and harmless.
Another essential step will be to use this baseline as a point of departure for exploring other techniques that
can better-address more advanced use cases and more speculative failure modes. New ideas and techniques
can then be pragmatically compared with existing methods, and then incorporated into standard practice if
they yield further improvements in safety and robustness. Our view is that the most relevant problems and
the most creative and effective alignment techniques will be identified and developed through research on
concrete AI systems. As we saw in Section 6.1, we are already encountering examples that point to the
limitations of human feedback, and so we need to begin to develop other methods.



7.2 Alignment Data as a Public Good   
In this work we allowed crowdworkers’ common-sense to define what constitutes helpful and harmless behavior. This was sufficient for our exploration of ‘technical alignment’, i.e. the question of whether certain
techniques can be used to train AI models to be more helpful and harmless. But we have avoided addressing
the underlying question of what sort of behavior should be expected from deployed AI models.
This question should not be the provenance of researchers only. That said, without a clear specification for the format and type of ‘alignment data’ most relevant for AI training, it has been difficult
for anyone other than researchers to gather the information needed to train safe and beneficial AI systems. However, recently several projects (including ours) have used similar methods [Stiennon et al., 2020,
Ouyang et al., 2022, Nakano et al., 2021] to teach AI models complex human preferences, and we have also
found [Askell et al., 2021] that preference modeling based on ranked comparisons scales better than many
other techniques.
One possible approach would be for an independent organization with ethical, legal, and cultural expertise to
create a very high-quality dataset expressing human preferences for AI behavior (via comparisons). Such an
organization could also use a novel governance structure, so that a larger set of societal stakeholders could
factor into the decisions it makes about how to create and curate alignment data – in contrast to today, where
private companies make these decisions in an opaque manner using governance structures that grant power
to financially interested parties. Datasets created in this way might be used for both training and evaluation
of AI models, and could even begin to establish standards for behavior. Due to the rapid improvement in AI
language models, we expect that such datasets would be most valuable if they encode preferences at humanlevel sophistication. In any case, this is just one speculative possibility for broadening participation in dataset
creation.
Our research has benefited from publicly available research datasets and evaluations relevant to aligning AI
with human values [Stiennon et al., 2020, Hendrycks et al., 2021a], and we plan to release our preference
modeling data for others to use in their research. Unfortunately, this does not seem to be a standard practice
among alignment researchers, as evidenced by some recent work. While we agree that LLMs themselves can
be used for harm, it seems that no such argument can be made for alignment data.
It’s extremely important to enable collaboration and reproducibility for alignment and safety research. As
AI systems become more powerful and more widely deployed, the cost of mistakes and misunderstandings
may grow immensely. We believe that the only way to convincingly address potential safety failures from
advanced AI systems is to build a thoughtful community of researchers with deep expertise, and the ability
19We view simplicity as essential, as an ad hoc, case-by-case treatment of AI failure modes will likely only treat visible
symptoms and create a false sense of security.
36
to evaluate systems empirically. This will remain almost impossible if knowledge about the alignment of
advanced systems remains siloed within many independent organizations. Sharing data seems like the easiest
and most commonsense way to enable the sharing and validation of results.
One ostensible reason for secrecy is that organizations may use data from users to develop alignment datasets,
and then justify not sharing the datasets on the grounds that it violates user privacy. This is a challenging
issue that requires organizations to think about how to reconcile commercial priorities with the need to create
a ‘safety commons’ for the community. If alignment becomes interlinked with the concept of commercial
moats, that could reduce the overall net level of safety of the AI ecosystem. Therefore, we believe that
datasets developed for alignment should be kept separate from commercial data, and should be openly shared
to advance research on safe and beneficial AI.



7.3 Broader Impacts    
We hope that our work provides compelling evidence that AI systems can be made safer and more useful at
the same time, and without performance costs. As noted above, we have largely remained agnostic on the
question of which values define acceptable and unacceptable AI behavior. Thus we hope that rapid progress
in technical alignment and the consolidation of specific techniques will motivate the development of publicly
available alignment data, guidelines, and benchmarks.
AI technologies are dual-use, meaning they can be used beneficially and otherwise. We have found the effectiveness of preference modeling and RLHF striking (in our research and others’), and believe there’s very
legitimate concern that these techniques could be used for censorship, fraud, and misinformation. Straightforward commercial use-cases also seem worrisome, especially if optimization for objectives like user engagement and persuasion are mixed together. At the most naive level, if you can optimize for ‘harmless’ then
you can ‘flip the sign’ and generate harmful systems.20 We also found that systems trained exclusively to be
helpful become easier to use for harmful ends, which suggests that as systems become more powerful, it will
become increasingly important to directly curb their potential for harms.
Perhaps the broadest impact of this work, and the general development and dissemination of controllable,
human-like language generation [Ganguli et al., 2022], will be cultural. In Figure 1 we used an Elo scale,
essentially the chess rating system, to compare and evaluate natural language assistants, and we even included
comparison to human writers. This kind of comparison risks trivializing the importance of language, which
is certainly not just a game, but the core medium of culture and society. While seeking to align increasingly
capable AI systems feels like a robustly good action, how and when to deploy these systems poses more
challenging questions – culture is fundamentally a human enterprise, but large-scale generative models hold
the possibility of magnifying and minimizing different parts of human culture in unpredictable and opaque
ways, which could have broad downstream influences.
 
