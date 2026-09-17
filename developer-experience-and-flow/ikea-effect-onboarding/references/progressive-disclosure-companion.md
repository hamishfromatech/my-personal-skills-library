# Progressive Disclosure: A Companion to the IKEA Effect

## Overview
Progressive Disclosure is a UX technique that defers advanced features and information to secondary UI components, keeping the primary interface minimal. When paired with the IKEA Effect, it creates a powerful dual strategy: users begin with a simple, welcoming experience, then gradually invest effort into customizing and building complexity as their competence grows.

## Why It Pairs With the IKEA Effect
The IKEA Effect requires effort, but effort can overwhelm beginners. Progressive Disclosure solves this by revealing complexity in stages:
1. **Stage 1 (Discovery):** Simple, low-friction entry. The user gets immediate value.
2. **Stage 2 (Customization):** The user is invited to personalize. Effort is introduced, but only after confidence is established.
3. **Stage 3 (Co-Creation):** Advanced features and community contributions. Maximum IKEA Effect activation.

## Application to A-Tech Projects

### A-Coder (The IDE)
- **Primary UI:** Clean code editor with basic AI assistance visible.
- **Progressive Layer 1:** User clicks "Configure AI" and chooses from preset personas (Student, Architect, Hacker). This is light customization.
- **Progressive Layer 2:** User enters "Advanced Mode" to set temperature, context window, local model endpoints, and custom system prompts. This is where IKEA Effect deepens.
- **Progressive Layer 3:** User exports their config as a shareable template or contributes a new AI Persona to the community repo. Full co-creation.

### Be Practical (The Book & Playbooks)
- **Primary Experience:** Read the core chapters. Low commitment.
- **Progressive Layer 1:** Download the fillable worksheet PDFs. Light interaction.
- **Progressive Layer 2:** Import the Playbooks into Notion/Obsidian and customize them for your workflow. Moderate effort, high ownership.
- **Progressive Layer 3:** Submit your customized playbook variant back to the community or share it in the Builder's Club. Maximum ownership and social proof.

### Open Source AI Builder's Club
- **Primary Experience:** Browse resources and read the welcome guide.
- **Progressive Layer 1:** Complete a "Getting Started" checklist.
- **Progressive Layer 2:** Deploy your first local model using Club tutorials. This is the critical IKEA Effect activation point.
- **Progressive Layer 3:** Write a tutorial, contribute to a repo, or mentor a new member. Full community ownership.

## Key UI Patterns for Progressive Disclosure
- **Accordions:** Hide advanced settings behind collapsible headers.
- **Tabs:** Separate basic from advanced functionality.
- **Modal Windows:** Open secondary configuration panels on demand.
- **Toggles:** Reveal developer or advanced features when enabled.
- **Staged Flows:** Step-by-step wizards that unlock features as the user progresses.
- **Responsive Enabling:** Gray out advanced options until prerequisites are met.

## Best Practices
1. **Keep Important Information Visible:** Never hide critical functionality. Only defer advanced or optional content.
2. **Limit Layers:** A single secondary screen is typically sufficient. Multiple nested layers frustrate users.
3. **Provide Clear Signifiers:** Use icons, arrows, and labels to indicate where advanced features live.
4. **Allow Reversibility:** Users should be able to undo or revert to simpler views without penalty.
5. **Contextual Help:** Use tooltips and prompts to guide users toward advanced features when they're ready.

## The "Dumbed Down" Risk
The biggest danger of Progressive Disclosure is oversimplifying to the point where advanced users feel constrained or the product appears shallow. The balance is to make the primary UI inviting while ensuring advanced users can find depth quickly.

## The Mental Model
Think of Progressive Disclosure as a curriculum and the IKEA Effect as the graduation project:
- **Curriculum:** Teaches without overwhelming.
- **Graduation Project:** Requires effort, creates pride, and produces something the student values deeply.

Combined, they turn a product from something users *try* into something users *built*.
