---
title: "Why did you think that was safe?"
date: 2026-08-28T08:42:10-07:00
draft: false
---

I've been reviewing OpenAI's [publications about the security incident where their models hacked Hugging Face](https://openai.com/index/hugging-face-incident-and-the-road-ahead/)
as they've come out. I have really appreciated the level of detail they've been willing to provide, but there's one detail that's felt under-discussed about this episode. The primary model involved was a "highly-persistent internal model":

> The Hugging Face intrusion involved two OpenAI models but was primarily driven by the activities of an internal-only research model trained to be highly persistent and diligent in its work. The GPT-5.6 Sol model was also involved

Thinking about the broader context of this incident -- that these highly capable models were running seemingly without much active supervision, at very large scales, with everyone involved aware of potential cybersecurity risks -- one question jumps out: **why did they think this was safe?**

This is often treated as a rhetorical question, and when asked straight-up it reads as an accusation. But it's essential if you want to understand how an incident actually happened. And it's even more essential if you want to prevent the next one.

A simple insight about accidents is that the people involved in them rarely intend to cause catastrophic failures. This is the Local Rationality Principle:

> In any situation, people will take the action that makes the most sense to them in the moment with the context they have

Like [I've talked about with blamelessness](/posts/why-do-we-do-blameless-incident-reviews/), this isn't to excuse bad operator behavior. However, it is a near-universal truth that failures are preceded by _normal_ operator behavior. Before taking an action that will lead to an outage, people assess risks and evaluate potential actions in the same way that they do in their normal work -- because they don't know in the moment that an outage is going to occur.

Very often, incident analysis misses this point. The way we often think about incidents is as a discrete series of events, leading inexorably towards catastrophe. It's incredibly difficult to shake this framing, because the events of an incident are so obviously abnormal in the aftermath.

{{< figure src="/dekker-counterfactuals.png" alt="" caption="A famous Sidney Dekker hindsight bias diagram" >}}

In reality, the people involved in incidents do not perceive them this way. The outcomes of decisions are not obvious; people must make high stakes cost-benefit decisions under time-pressure; a decision that clearly led to a failure in hindsight might have seemed like an important way to avoid a failure in the moment.

Incidents are so important as learning opportunities because they allow you to step back into the moment before the incident happened and understand:

- What even _is_ normal in your organization, a reality that invariably diverges from stated policy
- How people who are doing normal operations think about risk
- What the latent risks are that nobody was aware of before the incident that made catastrophe possible

In an [incident interview](https://howie-guide.pagerduty.com/interview/) or incident review, here's how I like to frame this question:

> If you think back to the moment before the incident occurred, what was your mental model of what "safe" meant here? What had you done to verify that the operation you were doing was safe?

When you start to adopt this view of incident causality, there are a few failure modes of conventional incident analysis that become really obvious. Post-incident action items are too focused on preventing "human error" and often restrict operators in ways that are unhelpful to normal operations and wouldn't have even prevented the incident in the first place. There's too much emphasis on monitoring and alerting, discounting what signals people actually pay attention to and not considering the amount of noise already present in the system. Actions taken immediately prior to the incident are always treated as abnormal, without attempting to set a reasonable baseline of normal behavior.

On that note, let's get back to OpenAI.

Something I have not seen reflected in their post-incident documentation is the fact that the Hugging Face hack occurred during normal model evaluation work.

_Worth emphasizing: I don't know what conversations they're having internally about this incident. Details about this type of thing are often omitted from public-facing incident reports. But maybe they shouldn't be!_

There is a reason that their researchers thought it was safe to run 1,200 concurrent instances of a "highly-persistent internal model" in a cybersecurity evaluation, without tracking what actions those instances were taking. They don't account for this reason, maybe because it's embarrassing. Given the choice, most companies would prefer to not tell their customers "we avoided implementing this safety mitigation because it was a known issue that had not bitten us yet, and we had more important things to do." We've all been there!

That's understandable, but it's problematic in this case because it makes it hard to understand how effective their post-incident plan of action will be. Many of these changes are clearly right, regardless: a defense-in-depth approach to sandbox security and a stronger focus on alignment are both on point to what we know happened. But especially when we're thinking about monitoring, it's hard to understand the adequacy of the solution without knowing how their day-to-day operations have changed.

You have to keep in mind that this incident happened during evaluation work that researchers perceived as safe, and affected a subsystem that no one anticipated would become an attack surface. Everyone is aware now that attacks on Artifactory are a risk during cyber exploit evaluations. But the next hack will not happen against Artifactory, during a cyber exploit evaluation. It will happen under different circumstances, when researchers doing normal work that they don't expect to cause a security incident. So the key question is: what did we learn about how to defend ourselves against these hacks **when we are not expecting them to happen?**

I don't feel confident saying with certainty that OpenAI is getting this wrong, again because we don't know what conversations they're having internally about this incident, but I do know that it's something that many software organizations routinely get wrong about high stakes outages. This one is just unusually high stakes.
