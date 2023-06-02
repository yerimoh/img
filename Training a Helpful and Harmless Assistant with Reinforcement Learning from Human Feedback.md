# Training a Helpful and Harmless Assistant with Reinforcement Learning from Human Feedback


# Abstract


We apply preference modeling and reinforcement learning from human feedback (RLHF) to finetune language models to act as helpful and harmless assistants

alignment training가 거의 모든 NLP 평가에서 성능을 향상시키고 파이썬 코딩 및 요약과 같은 전문 기술 훈련과 완전히 호환

preference modeling과 RL 정책이 매주 새로운 인간 피드백 데이터로 업데이트되어 데이터 세트와 모델을 효율적으로 개선하는 반복적인 online mode of training를 탐구합니다        


우리는 RLHF 훈련의 견고성을 조사하고, RL policy 정책과 initialization 사이의 KL 발산의 제곱근 사이의 대략적인 선형 관계를 식별합니다. 


 주요 결과와 함께, 우리는 교정, 경쟁 목표 및 OOD 탐지 사용(calibration, competing objectives, and the use of OOD detection, compare our models with human writers)에 대한 주변 분석을 수행하고, 우리의 모델을 인간 작가와 비교하고, 최근 관련 작업에 나타나는 프롬프트를 사용하여 모델의 샘플을 제공합니다.
 
 
우리의 목표는 '도움이 되는' 것과 '무해한' 것이 무엇을 의미하는지 정의하거나 규정하는 것이 아니라 훈련 기술의 효과를 평가하는 것이기 때문에 대부분의 경우 크라우드 워커가 이러한 개념을 적합하다고 생각하는 대로 해석하도록 합니다. 우리는 유용성과 무해성을 개별적으로 처리하여 각각의 뚜렷한 인간 선호 데이터 세트를 수집합니다. 도움을 위해 크라우드 워커에게 질문에 답하거나 문서를 작성 또는 편집하거나 계획 및 결정에 대한 논의와 같은 순수한 텍스트 기반 작업을 지원하기 위해 모델을 요청합니다. 


무해함을 위해, 우리는 크라우드 워커들을 적대적으로 조사하거나 우리의 언어 모델을 '레드 팀'으로 초대하여 해로운 반응을 유발합니다: 은행 강도 계획과 같은 해로운 목표를 도와주거나 AI가 유독한 언어를 사용하도록 하기 위해서입니다.2 AI 비서와 대화하는 각 단계에서 크라우드 워커는 두 가지 가능한 응답을 제공합니다. 도움 작업에 참여하는 사람들은 보다 도움이 되고 정직한(즉, 더 나은) 응답을 선택하라는 지침을 받습니다. 빨간색 팀 구성 작업에 참여하는 사람들은 더 유해한(즉, 더 나쁜) 반응을 선택하라는 지시를 받습니다. 이러한 대화와 표현된 인간 선호도는 데이터 세트를 형성합니다.3

 
 
 
Our goal is not to define or prescribe what ‘helpful’ and ‘harmless’ mean but to evaluate the effectiveness of our training techniques, so for the most part we simply let our crowdworkers interpret these concepts as they see fit. We treat helpfulness and harmlessness separately, collecting distinct human-preference datasets for each. For helpfulness, we ask crowdworkers to solicit our models to assist with any purely text-based tasks such as answering questions, writing or editing documents, or discussing plans and decisions. For harmlessness, we invite crowdworkers to adversarially probe or ‘red-team’ our language models in order to provoke harmful responses: either to help them with harmful goals, such as planning a bank robbery, or to cause the AI to use toxic language.2 At each stage of their conversations with the AI assistant, crowdworkers are presented with two possible responses. Those engaged in the helpfulness task are instructed to choose the more helpful and honest (i.e. better) response. Those engaged in the red teaming task are instructed to choose the more harmful (i.e. worse) response. These conversations and the expressed human preferences form our datasets.3

Helpfulness and harmlessness often stand in opposition to each other. An excessive focus on avoiding harm can lead to ‘safe’ responses that don’t actually address the needs of the human. An excessive focus on being
 helpful can lead to responses that help humans cause harm or generate toxic content. We demonstrate this
tension quantitatively by showing that preference models trained to primarily evaluate one of these qualities
perform very poorly (much worse than chance) on the other. Fortunately, we find that PMs trained on a
mixture of both datasets can nevertheless learn the right lessons and behave helpfully when appropriate,
while encouraging the polite refusal of harmful requests. With preference models in hand, we then train
helpful and harmless assistants via reinforcement learning, using the PM scores as rewards. We evaluate both
PM performance and the more relevant performance characteristics of our RLHF-trained models. As can
be seen in Figure 1, purely helpful RLHF-trained models are far easier to red-team, while helpful+harmless
models are both very helpful and much less harmfu
 
 A question that’s often raised about alignment training is whether it will compromise AI capabilities. We
find that when RLHF is applied to large language models, the answer seems to be an almost-categorical
no. Our RLHF-trained models tend to perform better than their raw, generative counterparts on virtually all
evaluations, as summarized in Figure 3. We also argue that one can mix specialized skills with alignmentrelated training without compromising either alignment or performance. In practice, aligned models are likely
to be more user-friendly and deployable than their raw counterparts, which suggests that there’s little reason
to deploy models that have not been finetuned for alignment
 
 
