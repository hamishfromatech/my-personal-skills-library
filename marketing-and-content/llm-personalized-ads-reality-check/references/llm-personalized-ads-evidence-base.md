# LLM Personalized Ads: Reality Check — Evidence Base

## Source
El Fraihi, Amieur, Roussillon & Goga (Inria / Institut Polytechnique de Paris / CNRS / Université Grenoble Alpes). "How Persuasive Are LLMs in the Wild? Assessing Personalized Ads in Real-World Delivery." ICWSM 2026 (Proceedings of the International AAAI Conference on Web and Social Media, Vol 20, Issue 1, pp 723-737).

## Experimental Design

### LLMs Used
- GPT-4o
- Gemini 1.5 Pro
- LLaMA 3.1

### Target Demographics
4 demographic groups (details in full paper)

### Personalization Strategies
1. **Tone and language adaptation**: Modifying linguistic style to match demographic preferences
2. **Audience-relevant themes**: Introducing topics/themes relevant to target demographic
3. **Selective emphasis**: Emphasizing specific elements from source material resonating with group

### Evaluation Dimensions
1. **User engagement**: live testing on Meta platforms (CTR, conversion, engagement)
2. **Perceived appeal**: user surveys
3. **Platform behavior**: algorithmic delivery analysis (demographic composition of actual vs intended audience)

## Key Findings

### Finding 1: No Significant Engagement Improvement
- Personalized messages did NOT significantly improve user engagement vs non-personalized alternatives
- For some demographic groups, specific personalization strategies REDUCED engagement
- This contrasts with controlled studies showing LLM personalization advantages

### Finding 2: Survey-Behavior Divergence
- Survey-based assessments of ad appeal diverged from observed behavioral outcomes
- Users may rate personalized ads as more appealing in surveys but not engage more in practice
- Implication: self-reported metrics are insufficient for evaluating LLM personalization

### Finding 3: Algorithmic Delivery Shift
- LLM-generated personalization cues shifted algorithmic ad delivery toward intended audience by up to 8%
- This occurred WITHOUT explicit targeting instructions
- The personalization language signaled audience relevance to platform's delivery algorithm
- Effect bounded by platform's own relevance predictions

## Context: Related LLM Persuasion Research

### Controlled Setting Findings (Positive)
- Simchon et al. (2024): ChatGPT-generated political ads congruent with Openness significantly more persuasive
- Matz et al. (2024): GPT-personalized messages outperformed generic baselines across consumer, political, health domains
- Bai et al. (2025, Nature Communications): LLM-generated messages can persuade humans on policy issues

### Mixed/Null Findings
- Hackenburg & Margetts (2024): Dynamically targeted AI-generated messages did NOT always outperform non-targeted
- Xu & Zhao (2026): Personality-label-based prompting had only limited effects

### Semantic Anchoring (Non-Ad Context)
Xu, Zhou & Zhao (Frontiers in Psychiatry, Jan 2026): LLM personality-tailored persuasion works when:
- Core content semantically anchored (functional backbone fixed, style varies)
- Topic stereotypes accounted for (baseline preferences often larger than personality effects)
- 3 studies, N=618, personality-matching effects in 16/18 conditions with anchoring
- Topic stereotypes: photography workshops (high Openness), music festivals (high Extraversion), donations (high Agreeableness), blind boxes (low Conscientiousness)
- LLMs process traits as stylistic signals, not motivational orientations

## Why Real-World Ad Personalization Fails Where Lab Studies Succeed

1. **Algorithmic mediation**: Platforms optimize for their own objectives, not yours
2. **Audience expectations**: Real users in natural browsing contexts differ from survey respondents
3. **Saturation**: Platform users see many ads; marginal personalization may not break through
4. **Delivery confounding**: The platform decides who sees the ad, confounding personalization effects
5. **Measurement gap**: Surveys measure perception; behavior measures action — different constructs

## Practical Evaluation Framework

### Three-Dimensional Assessment
1. **Engagement** (behavioral): CTR, conversion rate, engagement rate, cost per action
2. **Perceived appeal** (survey): persuasiveness, relevance, liking, willingness to engage
3. **Platform delivery** (algorithmic): actual audience demographics vs intended, delivery efficiency

### Decision Criteria
- If surveys say "appealing" but behavior says "no change" → personalization not working in practice
- If engagement improves but delivery shifts unexpectedly → platform algorithm confounding
- If all three align → personalization is effective

## Related Neuromarketing Context (2026 Research)

### AI-Enhanced Neuromarketing (Bucea-Manea-Țoniș et al., 2026)
- PLS-SEM study, N=416 Romanian university students/professors
- Neuromarketing knowledge → application (β=0.726, p<0.001)
- Application → marketing activities (β=0.555, p<0.001)
- Application → social media communication (β=0.633, p<0.001)
- AI amplifies through: real-time data processing, predictive analytics, automated personalization, social media optimization
- Ethical concerns: privacy risks, algorithmic bias

### Consumer Impulsivity & Neuromarketing (Nagpal et al., Frontiers in Psychology, March 2026)
- PLS-SEM, N=609, Delhi-NCR
- Significant: emotional appeals (β=0.469), cognitive processing (β=0.378), sensory triggers (β=0.173), neuro-pricing (β=0.134)
- NOT significant: scarcity/urgency (β=0.043), endorsement influence (β=0.045)
- Consumer traits moderate (β=0.075, p<0.001): trait-contingent effectiveness
- Key: affective + cognitively reinforced mechanisms, not scarcity/endorsement

## A-Tech Alignment
- **Open-source AI**: findings apply to any LLM (open or closed); LLaMA 3.1 tested alongside GPT-4o
- **Data privacy**: real-world platform data, privacy-protecting analysis
- **Financial freedom**: prevents overinvestment in ineffective personalization
- **Practical implementation**: real Meta ad deployment, behavioral metrics, not just surveys