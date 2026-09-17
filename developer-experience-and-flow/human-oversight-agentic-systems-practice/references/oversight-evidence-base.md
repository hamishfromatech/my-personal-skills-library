# Evidence Base: Human Oversight of Agentic Systems in Practice

## Source
Dhanorkar, S., Passi, S., & Vorvoreanu, M. (2026). Human oversight of agentic systems in practice: Examining the oversight work, challenges, and heuristics of developers using software agents. arXiv:2606.05391. Microsoft Research.

## Study Design
- **Method**: Qualitative, semi-structured interviews
- **Participants**: 17 developers (power users, early adopters)
- **Organizations**: 12 from one large tech company, 5 from other organizations
- **Experience**: 13 with 7+ years programming; 9 "experienced" with agents; 12 daily users
- **Data Collection**: July-August 2025, 60-minute interviews, $40 compensation
- **Analysis**: Three-round qualitative inductive analysis (open → axial → selective coding)

## Four Forms of Oversight Work

### 1. A Priori Control (Preventative)
**Goal**: Define clear boundaries, ensure proper setup, minimize failure

**Practices**:
- Configure agent autonomy settings
- Set deny lists (e.g., "never delete something")
- Supply global context via custom instruction files
- Maintain project scope documentation

**Challenges**:
- Developers perceive having little control over agent operation
- Limited information about agent reasoning and data policies
- Black-boxed agents prevent configuration
- Using prompts instead of custom instructions (despite后者 being more effective)

### 2. Co-Planning (Proactive)
**Goal**: Establish common ground, minimize misalignment, facilitate downstream oversight

**Practices**:
- Iterative prompting with refinement
- Drafting plans with agents (data flow, structure, key components)
- Seeding partial solutions / code snippets
- Task decomposition into small, testable chunks with minimal side effects

**Key Insight**: "Do not completely go hands-off" — LLMs have short-term memory; without guidance, they make inconsistent decisions across the project

**Challenges**:
- Difficulty identifying appropriate specificity level
- What's "obvious" to humans is invisible to agents
- Natural language limitations for articulating complex goals
- "Making the prompt coherent is like writing a story" — hard to construct in 15 seconds

### 3. Real-Time Monitoring (Reactive)
**Goal**: Ensure agents remain on track, not stuck in loops, follow plans

**Reality**: Rarely performed — most developers never "proactively look at the thought process"

**Practices (when done)**:
- Observe action logs perfunctorily ("eyeball to see it did the right thing")
- Use ad hoc cues (abnormally long run time, conversation turns)
- Pause and stop agents mid-execution

**Challenges**:
- Disconnect between agent statements and actual actions (10% of time agent is wrong about why it did something)
- Difficulty fixing agent mistakes mid-execution
- "If you try to redirect, it doesn't get back on track. I have to stop and restart"

### 4. Post Hoc Review (Evaluative)
**Goal**: Ensure agent outputs are correct and achieved via appropriate approaches

**Practices**:
- Review code outputs using diff tools
- Use LLMs-as-a-judge for evaluation
- Layered testing (agent-generated test suites + manual testing)
- Read every single line for serious software engineering

**Challenges**:
- Cognitive distance: "Because the code is not created by me, my understanding is very surface level"
- Re-review burden: "Each time you say 'fix it again,' you have to review it again"
- Volume: "The sheer volume of agent-generated code is so high"
- Even small changes require significant time to understand unfamiliar code

## Four Oversight Heuristics

### Heuristic 1: Plan-as-Proxy
**Assumption**: Agent's plan faithfully represents its actual working
**Practice**: Substitute code review effort with easier plan verification
**Evidence**: "I don't need to understand the whole thought process... The only thing that matters is the end: what did you do?"
**Risk**: Agents may deviate from plans during execution

### Heuristic 2: Test-Results-as-Guarantee
**Assumption**: Passing test results guarantee code correctness
**Practice**: Outsource verification to test suite; review test results instead of code
**Evidence**: "If AI is able to pass all test cases and build, then we give it a go and we don't even need to take a look at the source folder"
**Risk**: Depends entirely on test quality; tests may not cover edge cases

### Heuristic 3: Eyeballing
**Assumption**: Spot-checking suffices as incomplete-yet-efficient information processing
**Practice**: Skim agent outputs, rationales, change summaries; request visual diagrams
**Evidence**: "I have this way of doing spot checks or high-level checks"
**Risk**: Subtle errors can slip through; not rigorous enough for complex changes

### Heuristic 4: Trust-by-Necessity
**Assumption**: Reasonable to trust agents with new information or unfamiliar contexts
**Practice**: Defer to agent expertise when lacking own knowledge
**Evidence**: "Sometimes there are very technical details... I usually will have to trust the model"
**Risk**: Automation bias; epistemic deference may lead to accepting incorrect outputs
**Variant**: Social proof — using two different agents on same task increases confidence

## Key Takeaways

### Oversight ≠ Just Reviewing and Monitoring
Oversight is also **preventative** (configuring before prompting) and **proactive** (co-planning before execution). Oversight work begins before the prompt.

### Practical Heuristics over Idealized Oversight
Developers opt for "good-enough" supervision driven by bounded rationality. Plans become proxies, test results stand in for correctness, skimming replaces deep review.

### Developer-Manager Role Emergence
Traditional "craftsman" model → "developer-manager" where hands-on coding is secondary to oversight work. Core competency shifting from syntax mastery to architectural design, critique, review.

### EU AI Act Implication
Article 14(4)(a) requires developers working with high-risk AI systems to have situational awareness and epistemic access to resist automation bias.

## A-Tech Alignment
- **Open-source AI**: Applies to open-source agents (Cline, OpenHands, Aider)
- **Data privacy**: Oversight ensures sensitive data not leaked to agents
- **Financial freedom**: Effective oversight reduces wasted development from agent errors
- **Practical implementation**: 17 real-world developers, qualitative depth

## Cross-References
- `agent-experience-ax-devex-evolution` — AX design tenets
- `coding-agent-misalignment-large-scale` — misalignment taxonomy
- `agentic-cognitive-engagement-decline` — engagement decline patterns
- `agent-oversight-work-heuristics` — oversight heuristics (related)
- `proactive-ai-workflow-boundary-timing` — intervention timing