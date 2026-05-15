# Human-Like Writing AI Skill

This repository contains the `human-like-writing` skill. This skill serves as a prompt and behavioral override for AI models, forcing them to generate text that is virtually indistinguishable from human writing. It combats the predictable, monotonous, and cliché-ridden nature of standard AI outputs.

## Core Concepts

The skill is built around manipulating two technical writing metrics:

1. **Perplexity (Predictability of Word Choice)**
   * **AI Default:** Low. The AI chooses mathematically probable words, leading to predictable and common prose.
   * **Skill Override:** High. The skill forces the AI to use unexpected adjectives, highly specific verbs, and avoid overused phrasing (like "delve into" or "tapestry").

2. **Burstiness (Variation in Structure)**
   * **AI Default:** Low. The AI produces uniform, rhythmic sentences that maintain a steady tempo.
   * **Skill Override:** High. The skill forces dramatic variance in sentence length, alternating between 1-5 word "punchy" sentences and complex, winding ones.

## How to Use

When making a request to the AI, simply reference the skill:
> "Write a blog post about the importance of sleep. Use the `human-like-writing` skill."

Or apply it implicitly when setting up a custom agent.

---

## Examples of the Skill in Action

### Example 1: The Corporate Overview

**❌ Standard AI Generation (Robotic)**
> "In today's ever-evolving digital landscape, it is essential for modern businesses to leverage new technologies to enhance productivity. Furthermore, integrating AI tools offers a rich tapestry of solutions that can seamlessly streamline workflows. Ultimately, adapting to these dynamic changes is a testament to a company's resilience and forward-thinking ethos."

**✅ Human-Like Writing Skill Output**
> "Tech moves fast. Frankly, trying to keep up with every new platform or app can be exhausting. But here's the reality: you don't need to adopt everything. You just need to find the specific tool that stops your team from wasting three hours on a Tuesday afternoon. That's the real shift. Not a digital revolution, just getting your time back."

### Example 2: The Apology Email

**❌ Standard AI Generation (Robotic)**
> "I am writing to sincerely apologize for the delay in delivering the final report. Due to unforeseen circumstances, our timeline was unexpectedly extended. Please rest assured that we are taking the necessary steps to rectify this situation. We highly value your continued partnership and appreciate your patience during this time."

**✅ Human-Like Writing Skill Output**
> "I'm incredibly sorry about the delay on the final report. We hit an unexpected snag with the data processing last night, and it threw off the timeline completely. It's frustrating, I know. I'm actively working to get this wrapped up and in your inbox by tomorrow morning. Thanks for bearing with us on this one."

### Example 3: The Marketing Pitch

**❌ Standard AI Generation (Robotic)**
> "Elevate your morning routine with our premium, artisan-roasted coffee blends. Crafted to perfection, our beans provide an unparalleled symphony of flavors that will invigorate your senses. Dive into a world of exquisite taste and supercharge your day with unmatched energy."

**✅ Human-Like Writing Skill Output**
> "Look, mornings are hard enough. The least you deserve is a cup of coffee that doesn't taste like cardboard. We don't do 'symphonies of flavor.' We just roast really, really good beans so you can wake up, take that first sip, and actually feel human again."

---

## Technical Details

The AI achieves this by cross-referencing a strictly enforced "Banned Words" list (which includes phrases like *furthermore*, *leverage*, and *delve*) and running a mental self-check on its structural rhythm before finalizing its output. By breaking the metronomic rhythm and introducing conversational imperfections, the AI successfully mimics natural human expression.
