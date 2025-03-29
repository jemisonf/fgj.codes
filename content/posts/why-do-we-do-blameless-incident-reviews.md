---
title: "Why do we do blameless incident reviews?"
date: 2025-03-29T10:00:00-07:00
draft: false
---

Blamelessness is a widely accepted and poorly understood practice in the software industry. It was [introduced by Etsy engineers](https://www.etsy.com/codeascraft/blameless-postmortems/) with a deep background in resilience engineering and a desire to improve safety in their organization, but the roots of blamelessness in safety research have been lost as the concept has made its way around the world. Today, software engineers routinely misunderstand the value of blamelessness and practice it in ways that are sometimes actively counterproductive. Let’s talk about why.

There are two primary benefits to practicing blameless incident reviews:

1. Creating a “just culture” in your organization  
2. Actually understanding your incidents

The first benefit, from what I’ve seen, is what most engineers are familiar with. You may not know the term “just culture” (see the article linked in the first paragraph) but it encapsulates the idea that people should be treated fairly when they’re involved in incidents, and that doing so has a lot of organizational benefits. Some of these include: avoiding undue risk-aversion in engineers, making it easier to speak freely about the events of an incident, and encouraging teams to freely participate in the post-incident process. This is generally how the incident is framed in [the articles you find if you google “why do blameless postmortems”](https://www.atlassian.com/incident-management/postmortem/blameless). This isn’t wrong, but it’s incomplete.

In organizations that over-index on this concept of blameless, you see a couple of common anti-patterns:

* Senior engineers are comfortable blaming themselves for incidents they participated in, understanding that they won’t be punished for their actions  
* Blame shifts from individuals to teams, since diffused blame implies less individual responsibility  
* People assume mistakes *just happen*, and don’t discuss the details of participants’ actions during an incident. If you use the passive voice a lot in your incident writeups you are probably doing this  
* Incidents are attributed to “human error”, while the human who committed the “error” is not personally blamed

To use some safety jargon, this is what happens when you apply blamelessness in a [Safety-I culture](https://www.england.nhs.uk/signuptosafety/wp-content/uploads/sites/16/2015/10/safety-1-safety-2-whte-papr.pdf). Safety-I assumes that normal operations are safe, accidents (or incidents, in software terms) happen because of abnormal operations, and abnormal operations happen because of human error.

If it feels oxymoronic to prevent yourself from talking about “human error” in a safety culture where “human error” is the main cause of incidents, that’s because it is. It’s not surprising that a lot of people chafe at this particular iteration of blamelessness; honestly, it’s surprising more people *don’t* question it. Paraphrasing an engineer I spoke with recently at SREcon:

> When we do blameless incident reviews, it’s like we can’t actually talk about what happened in the incident. On teams that don’t do blamelessness, you might get yelled at but at least you can actually talk about what you did.

If you want to correctly practice blamelessness, we need to understand the second reason it exists: because it’s the only way to understand your incidents. 

This involves a key principle of Safety-II thinking: systems constantly run in a degraded mode, humans constantly adapt to changing circumstances, and accidents happen when normal operations collide with environmental factors that make failures possible. Put a little more simply: people generally don’t do things they think will cause accidents, so people’s actions are rarely sufficient to explain why incidents occurred.

Put even more simply: human error doesn’t exist.

An anecdote from Sidney Dekker’s *Field Guide to Human Error Investigations* lays this out well:

> Someone called me on the phone, demanding to know how it was possible that train drivers ran red lights. Britain had just suffered one of its worst rail disasters—this time at Ladbroke Grove near Paddington station in London. A commuter train had run head-on into a high-speed intercity coming from the other direction. Many travelers were killed in the crash and ensuing fire. The investigation returned a verdict of "human error". The driver of the commuter train had gone right underneath signal 109 just outside the station, and signal 109 had been red, or "unsafe". How could he have missed it? A photograph published around the same time showed sensationally how another driver was reading a newspaper while driving his train.

> The Ladbroke Grove verdict of "driver error" lost credibility very soon after it came to light that signal 109 was actually a cause célèbre among train drivers. Signal 109 and the entire cluttered rack on which it was suspended together with many other signals, were infamous. Many drivers had passed an unsafe signal 109 over the preceding years and the drivers' union had been complaining about its lack of visibility. In trains like the one that crashed at Ladbroke Grove, automatic train braking systems (ATB) had not been installed because they had been considered too expensive. Train operators had grudgingly agreed to install a "lite" version of ATB, which in some sense relied as much on driver vigilance as the red light itself did.

If you actually speak to incident participants who “caused” incidents about their involvement, a story you will almost always hear is:

1. I was following a normal procedure for doing something \[where the “normal” procedure is rarely the “official” procedure\]  
2. I did enough checks to satisfy my understanding of what could potentially go wrong with this action  
3. I did the thing, and stuff started breaking

Based on that information, we want to identify a couple of critical details about the person’s actions:

1. What is missing from the official procedure such that operators are required to deviate from it?  
2. What was insufficient about the operator’s understanding of the system that they weren’t able to prevent the incident?  
3. What environmental factors made it possible for the incident to occur?

When you decompose the problem into its constituent parts, “human error” tends to disappear, and performance of individual actors in the incident becomes uninteresting. If you were to focus on “mistakes” made by the people involved, you’d miss all the parts of the incident that actually matter to the rest of your organization.

I think it’s important to call out that in a safety-II culture that practices blamelessness this way, there is no conflict between blamelessness and accountability. Rather than blamelessness allowing people to dodge accountability for “mistakes” (a thing that can genuinely happen in safety-I cultures) blamelessness becomes the *only* way a team can hold themselves accountable for incidents because it allows them to understand how they actually operate and how they can make durable changes to prevent future incidents.

If you want to move your organization towards a blameless safety-II culture, the most important thing you can do in incident reviews is to have people speak directly about their own involvement in the incident. The review facilitator (or the author of a written document) should focus the conversation on the mental model of the person involved in the incident and help everyone understand the actual process that they were following prior to the incident. I highly recommend the [Howie Guide](https://howie-guide.pagerduty.com/) and the [Etsy Debriefing Facilitation Guide](https://extfiles.etsy.com/DebriefingFacilitationGuide.pdf) as background for these discussions. 

*If you’ve made it this far in this post, you probably also owe it to yourself to read through [the Field Guide to Human Error Investigations](https://www.humanfactors.lth.se/fileadmin/lusa/Sidney_Dekker/books/DekkersFieldGuide.pdf).*

You should also be careful about practices that diffuse blame rather than facing it head on. Never use the passive voice to describe someone’s action in an incident (“the change was deployed”), and if you feel like a matter of fact description an action makes someone look bad that’s almost always a sign that you should surround that description with more context about the process someone was following. Smart people disagree about if you should use the names of individual engineers in incident review *documents*, but even if you are going to avoid people’s names you should be careful to not also strip out human agency from your document. 

In general, I’d also encourage people to engage more critically with this type of industry “best practice”. Software engineering culture isn’t very literate; we tend to distribute knowledge through conference talks, blog posts, and folk knowledge rather than papers and books, and that ultimately means that big ideas tend to quickly lose the context in which they were introduced. This post is a couple years in the making, and is the product of me asking questions about blamelessness and having trouble overcoming many of the objections that blameless-skeptics often raise. Understanding the safety research roots of the concept is what finally made me click, and it’s made me a much better incident analyst overall.
