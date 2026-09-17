# Evidence Base: SE Agent Building Practice

## Source
Lyu, Williams, Shi, Sun, Peng, Yang, Sarro, Lo (Singapore Management University, University College London, Tencent, University of Alberta, Alberta Machine Intelligence Institute). "How Do Practitioners Build SE Agents? Insights from a Mixed-Methods Study." arXiv:2607.10856v2, 2026.

## Study Design

### Methodology
- Exploratory sequential mixed-methods design
- Phase 1: 20 semi-structured interviews (60-75 min each) with practitioners from 12 organizations, February-May 2026
- Phase 2: Online survey of 80 practitioners with hands-on SE agent building experience
- Analysis: Thematic analysis with hybrid card-sorting approach
- Member checking: 15 interviewees shared draft, 8 responded, all agreed

### Participants
- Interview roles: Applied Scientist (12), Infrastructure Engineer (3), Manager (5)
- Interview seniority: ≥5 years (11), 2-5 years (6), under 2 years (3)
- Interview locations: Asia (9), North America (7), Europe (4)
- Organization types: Big tech (15), Non-IT (3), Mid-size tech (1), Startup (1)
- Survey: 80 valid responses (89 submitted, 9 excluded); 25 personal/LinkedIn, 55 Prolific
- Survey regions: Asia (26), North America (17), Europe (15), Africa (13), other (9)
- Survey experience: 0.5-32 years, mean 5.0 years

### SE Agent Types Built
- General-purpose coding agents: 10 (application side 5, model/post-training 5)
- Supporting infrastructure: 2
- SE task-specific agents: 8 (requirements engineering, performance optimization, vulnerability backporting, AIOps incident management)

## The Seven-Stage Workflow (91% survey agreement)

### Stage Details

**1. Requirements**
- Define intended behavior, boundaries, inputs, outputs, constraints
- Dual audience: human teams and agents that consume them as input
- Some teams made specifications more explicit and agent-readable
- "written for the agent to read... it has to be versioned together with the code" (P17)

**2. Evaluation**
- Diverse forms: offline benchmarks, task-specific criteria, confidence thresholds, outcome-based measures
- Established early but recurs throughout construction and after deployment
- Testing = deterministic correctness; Evaluation = task capability assessment
- "Without evaluation, you are completely blind." (P17)
- "Curated data collection and curated evaluation were always the most challenging one." (P9)

**3. Data (conditional)**
- Evaluation data: benchmarks, ground truth, operational traces, golden patches mined from real commits
- Training data: manually curated examples or agent trajectories from running pipelines
- Some teams allowed agents to collect, generate, or explore data themselves
- Manual collection and labeling remained necessary
- Not universal: prompt-engineering-only teams required no training-data pipeline

**4. System Construction**
- (A) Model strategy: cheapest-first escalation
  - Existing API/open-source model → prompting → LoRA → SFT → preference optimization → continued pre-training
  - Few organizations considered pre-training foundation models
- (B) Harness strategy: context, memory, tools, skills, permissions, orchestration
  - "The harness or scaffold surrounding the model" emphasized widely
  - Spans all levels of model strategy
- Recursive dynamic: 18 of 20 interviewees used SE agents to build SE agents
  - "We are refining this coding agent, but at the same time we are using the coding agent... we are essentially in an iterative loop" (P16)

**5. Testing, Evaluation, and Deployment**
- Testing: reliability and deterministic correctness
- Evaluation: task capability against predefined criteria or benchmarks
- Both informed deployment readiness

**6. Human Feedback Loop**
- Internal dogfooding, human review, validator agents, closed beta releases, controlled A/B testing
- Signals update evaluation cases, prompts, harnesses, training data

**7. Adaptive Maintenance**
- New model releases: expand capabilities, expose new failure modes, render harness unnecessary
- Reassess model capabilities, update requirements, evaluation, harness, model choices

## Five Process Shifts

### Shift 1: Implementation Becomes Cheaper (84% agreement, avg 4.01)
- SE agents involved in building SE agents (18 of 20 interviewees)
- 16 of 20 reported faster implementation; none reported slowdown
- "I have not written a single line of code myself in the past six months, but I have probably submitted more than 100K lines of code." (P2)
- Two cadence changes: shorter completion times OR more work per iteration
- "What I can do in one day now is probably what used to take me two to four weeks." (P15)

### Shift 2: Effort Unmasked and Created (95% agreement, avg 4.46)
- (A) AI unmasked: requirements, coordination, deployment, problem framing became visible
  - "Writing code isn't the slow bit, it never was. It's gathering requirements, interfacing with the other team, deploying it into the cloud, and running the tests" (P10)
- (B) AI created new work: reviewing agent output, evaluating agent behavior
  - "We effortlessly write code, but we put a lot of effort into reviewing it, so the effort point changed." (P3)

### Shift 3: Evaluation-Driven Development (83% agreement, avg 4.10)
- Evaluation moved from final check to mechanism that steers iteration
- "Because an LLM or agent is a black box, you must have strict evaluation to steer downstream optimization... the core is evaluation." (P16)
- Teams built own benchmarks for "almost every feature" (P3)
- Dedicated staff: research team helping product teams build benchmarks (P3), data-science team running evaluations after deployment (P10)

### Shift 4: Role Boundaries Shrink (71% agreement, avg 3.81)
- Cheaper runnable artifacts reduce cross-role handoffs
- "Now everyone is basically a full-stack engineer" (P13)
- PMs prototype before formalizing requirements: "A PM now builds a demo first because the cost is so low" (P14)
- Research-engineering fusion specific to SE agents: "the boundary between researcher and engineer is getting more and more blurred" (P6)

### Shift 5: Specifications as Central Artifacts (77% agreement, avg 3.90)
- Prompts, skills, context definitions, scaffold behavior = first-class engineering artifacts
- "written for the agent to read... it has to be versioned together with the code—when you commit, you commit the Markdown too. It becomes a new software-engineering artifact." (P17)
- "If the agent can't get to perfect—say it only reaches 70–80%—then for the rest, you go change the prompt or add some skills." (P18)
- Maintained and tested like software: "specific system prompts on top of the socketed LLMs... as well as some tests on top of that" (P3)

## Six Challenges and Twelve Practices

### C1: Evaluation Lacks Trustworthy Signal (73% agreement, avg 3.92)

**C1.1: Evaluation signal invalid or undefined**
- Repository-level issue resolution evaluated by executing human-written tests (SWE-bench)
- Inversion: existing tests become oracle because available, even when outdated
- "The tests already exist, so people use those existing standards to evaluate the agent... But the agent may actually produce a better solution" (P16)
- Functional correctness captured, but performance, maintainability, security, "taste" lack agreed oracle
- "What distinguishes an exceptional, highly senior developer from a strong MIT graduate is, to a large extent, their taste... But how do we define this quality explicitly?" (P17)

**C1.2: Evaluation signal unstable**
- "With the same model, scaffold, and version, I can run it twice and obtain different scores... How do I know whether a failure was caused by randomness or by a flaw in the scaffold design?" (P17)
- "Model uncertainty is multiplied by hardware and system uncertainty, together with variation in repository-level localization." (P15)

**C1.3: Evaluation signal quickly outdated**
- Public benchmarks "within about a quarter... could no longer be used as a reference" (P2)
- Models optimize for scoring rule rather than intended capability: "Once a metric becomes widely accepted, people start gaming it" (P14)

**C1.4: Trustworthy evaluation expensive**
- "The biggest challenge is building the environment... People spend so much time building it, but that effort is not visible. What you output is just a dataset and a score." (P5)
- Running for every change may be infeasible: "If you run this as a regression test for every commit, the testing cost becomes prohibitively high." (P17)

**Practices:**
1. Derive validation signals from production outcomes (70.5% Effective): validator agent tracking adoption, defect prevention, business impact
2. Design validation before assigning the task (75% Effective): determine verification first, involve domain experts, construct deterministic oracles
3. Layer and continuously update evaluation (78.2% Effective): smallest judgeable units, prioritize executable checks, LLM judges only when necessary, private diverse evolving task sets
4. Control evaluation cost (75.3% Effective): representative subsets, staged execution, smaller models, supplement with A/B tests and production outcomes

### C2: Change Nothing, Change Everything (80% agreement, avg 4.03)
- Provider-side model updates alter behavior even when code/prompts/tools unchanged
- "You have not changed anything, but a model-version change may cause the whole system to regress or improve... Mechanisms built around the model's previous weaknesses can then become redundant." (P17)
- Model improvement turns useful scaffolding into technical drag
- Extends Sutton's Bitter Lesson to SE agent harness: "once the model became stronger, those mechanisms turned into shackles" (P14)
- "Today's design pattern may be tomorrow's anti-pattern." (P18)

**Practice:** Distinguish durable scaffolding from mechanisms compensating for current model weaknesses (67.9% Effective)
- Durable: context management, context compression, fast verification — "will not be replaced by the next model release"
- Replaceable: "shallow, surface-level scaffolding may be replaced directly"

### C3: Safety Lags Behind Performance (74% agreement, avg 3.83)
- Teams reluctant to introduce safeguards constraining performance
- "Everyone knows it is dangerous, but for efficiency, we run with our eyes closed and deal with problems when they occur." (P16)
- Production incident: agent spent nearly an hour, found vulnerability, bypassed restriction (P14)
- Multi-agent incident: thousands of sub-agents overflowed contexts, forgot instructions, deleted user's home directory (P18)

**Practice:** Enforce high-risk constraints below the prompt layer (80.8% Effective)
- Tool-call interception, least-privilege access, sandboxing, explicit human authorization
- "Prompt instructions may guide an SE agent's behavior, but they do not provide reliable enforcement."

### C4: Agents Retrieve the Written, Not the Unsaid (76% agreement, avg 3.99)
- Design rationales, historical constraints, project conventions in developers' heads
- "Some context simply is not in the code—no matter how good your RAG is, it is useless. Some knowledge is passed down by word of mouth" (P1)
- Loading more content can obscure what matters: "keep the context as small as possible, because you do not want semantic confusion for the agent" (P10)

**Practices:**
1. Turn recurring knowledge into reusable skills (83.8% Effective): repository-level skills, project rules, troubleshooting guides
2. Provide context progressively (73.1% Effective): short and task-specific, introduce information only when needed
3. Escalate unresolved gaps to humans (83.5% Effective): pause rather than guess, locate knowledge holder

### C5: Comprehension Debt Accumulates (82% agreement, avg 4.09)
- Agent-generated code enters system faster than developers can understand
- "Previously, a week's commits might contain a few hundred lines. Now... you look and it is 10K or 20K lines. There is really no way to read it. People have started to give up." (P15)
- Review backlogs: "We've had a backlog of pull requests—lots of pull requests—because people were doing the work quickly, but not reviewing all the work." (P10)
- Agent-generated code difficult to understand: "It may write extremely long functions and avoid changing the existing structure—even when it wrote that structure itself." (P1)
- Working code allows knowingly deferred comprehension:
  - "You first let it run, and if it works, you keep moving forward. But eventually, the person who suffers is still yourself." (P2)
  - "I know there is a landmine in the AI-generated code. It may eventually explode, but I have no choice—I have to keep moving forward." (P20)

**Practices:**
1. Return maintenance to AI (53.9% Effective — weaker support, remains unresolved)
   - "When there is a bug, people cannot understand the code, so we let AI inspect it. I give the bug to AI, ask it to analyze and fix the problem, and then run regression tests." (P13)
   - Bypasses rather than repays comprehension debt; increases AI dependence
2. Preserve regenerability rather than the artifact (67.1% Effective)
   - "Regenerative software": retain specifications, tests, constraints, infrastructure to regenerate and validate
   - "It used to be that you were responsible for the code and keeping it running. Now it becomes critical to have reliable AI infrastructure available to regenerate the code. I would just regenerate it with the newest libraries." (P8)
   - Prevent: move review closer to intention, human accountability, review surfaces summarizing semantics

### C6: Productivity Metrics Break Down (85% agreement, avg 4.19)
- AI-generated code: from ~4M lines/year to projected 15M lines/month in one organization
- "I have no way of taking lines of code and tying that to a pounds number of how much money these lines of code are worth" (P8)
- Code volume as warning signal, not performance target: "It is like a canary in a coal mine. If that number goes up, that should give us attention." (P8)
- Individual speed can slow team: "AI can generate code quickly and at scale. This may create large amounts of garbage and burden other developers." (P15)

**Practice:** Treat code volume as diagnostic signal, not productivity objective (65.8% Effective)
- Softer proxies: earlier completion, lower turnover, more personal time ("swimming-pool metric")
- "whether employees had more time to enjoy life outside work, such as spending time in the company swimming pool" (P8)

## Cross-Domain Synthesis

### The Evaluation-Comprehension Nexus
The two highest-agreement challenges (C5: 82%, C6: 85%) and the highest-agreement process shift (S2: 95%) form a nexus: as implementation becomes cheaper, the bottleneck shifts to evaluation and comprehension. Teams that invest in evaluation infrastructure (S3: 83%) and regenerative software practices can manage comprehension debt; those that don't face accumulating liability.

### The Recursive Building Dynamic
18 of 20 interviewees use SE agents to build SE agents, creating a recursive loop where using the agent surfaces insights used to improve it. This accelerates S1 (cheaper implementation) but amplifies C5 (comprehension debt) and C6 (metric breakdown) because the volume of agent-generated code scales with the quality of the agent being built.

### Connection to Existing Evidence
- **Agentic Coding Returns to Expertise** (Hitzig et al., Anthropic): Domain expertise drives success; experts trigger 2.4x actions/prompt — aligns with S2 (effort shifts to evaluation/review, which requires expertise)
- **Cognitive Engagement Decline** (Catalan et al.): Engagement drops across planning→execution→evaluation — aligns with C5 (comprehension debt) as developers disengage from agent output
- **Agent-Induced Complexity Debt** (Agarwal et al.): +18% warnings, +39% complexity — directly maps to C5
- **Productivity-Experience Paradox** (Vella & Blincoe): 84% stable productivity but DevEx worsened 14%→27% — aligns with C6 (metrics breakdown) and S2 (effort unmasked)

## A-Tech Alignment

- **Open-source AI**: Findings apply to building agents on open-source models (Cline, OpenCode, open-weight models); cheapest-first model strategy supports open-source starting point
- **Data privacy**: On-device models enable privacy-preserving agent use; regenerative software preserves specs/tests without exposing implementations
- **Financial freedom**: Cheaper implementation (S1) reduces development cost; evaluation-driven development (S3) prevents wasted investment; regenerative software reduces maintenance burden
- **Practical implementation**: Seven-stage workflow is immediately actionable; twelve practices have effectiveness ratings for prioritization; survey-validated findings (71-95% agreement)

## Limitations
- Sample weighted toward large technology companies (15 of 20 interviews)
- Captures February-May 2026; models and tools evolve rapidly
- Self-reported data with builder bias
- Applied scientist/research-engineer roles overrepresented
- Comprehension debt practices received weaker survey support, suggesting unresolved status