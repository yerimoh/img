# Training a Helpful and Harmless Assistant with Reinforcement Learning from Human Feedback


# Abstract


We apply preference modeling and reinforcement learning from human feedback (RLHF) to finetune language models to act as helpful and harmless assistants

alignment training가 거의 모든 NLP 평가에서 성능을 향상시키고 파이썬 코딩 및 요약과 같은 전문 기술 훈련과 완전히 호환

preference modeling과 RL 정책이 매주 새로운 인간 피드백 데이터로 업데이트되어 데이터 세트와 모델을 효율적으로 개선하는 반복적인 online mode of training를 탐구합니다        


우리는 RLHF 훈련의 견고성을 조사하고, RL policy 정책과 initialization 사이의 KL 발산의 제곱근 사이의 대략적인 선형 관계를 식별합니다. 


 주요 결과와 함께, 우리는 교정, 경쟁 목표 및 OOD 탐지 사용(calibration, competing objectives, and the use of OOD detection, compare our models with human writers)에 대한 주변 분석을 수행하고, 우리의 모델을 인간 작가와 비교하고, 최근 관련 작업에 나타나는 프롬프트를 사용하여 모델의 샘플을 제공합니다.
 
 
 
 Our goal is not to define or prescribe what ‘helpful’ and ‘harmless’ mean but to evaluate the effectiveness
of our training techniques, so for the most part we simply let our crowdworkers interpret these concepts as
they see fit. We treat helpfulness and harmlessness separately, collecting distinct human-preference datasets
for each. For helpfulness, we ask crowdworkers to solicit our models to assist with any purely text-based
tasks such as answering questions, writing or editing documents, or discussing plans and decisions. For
harmlessness, we invite crowdworkers to adversarially probe or ‘red-team’ our language models in order to
provoke harmful responses: either to help them with harmful goals, such as planning a bank robbery, or to
cause the AI to use toxic language.2 At each stage of their conversations with the AI assistant, crowdworkers
are presented with two possible responses. Those engaged in the helpfulness task are instructed to choose the
more helpful and honest (i.e. better) response. Those engaged in the red teaming task are instructed to choose
the more harmful (i.e. worse) response. These conversations and the expressed human preferences form our
datasets.3


Helpfulness and harmlessness often stand in opposition to each other. An excessive focus on avoiding harm
can lead to ‘safe’ responses that don’t actually address the needs of the human. An excessive focus on being
 
 
 
 
 
 
 
 
