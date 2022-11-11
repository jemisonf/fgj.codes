---
title: "AWS App Runner is really cheap?"
date: 2022-11-06T11:51:37-08:00
---

I was playing around with AWS App runner recently and learned two things that I hadn’t previously known about its pricing. The first is from the pricing page:

> When your application is deployed, you pay for the memory provisioned in each container instance. Keeping your container instance's memory provisioned when your application is idle ensures it can deliver consistently low millisecond latency
> 

> When your application is processing requests, you switch from provisioned container instances to active container instances that consume both memory and compute resources
> 

The second, not mentioned on the pricing page, is that you don’t have to pay for an [Elastic Load Balancer instance](https://aws.amazon.com/elasticloadbalancing/pricing/) when using App Runner. 

As someone who uses [AWS Elastic Container Service](https://aws.amazon.com/ecs/) a lot professionally but avoids it for personal stuff because of cost, those are both a pretty big deal:

- Not having to pay for a load balancer saves you a flat $15 or so per month
- You can’t scale an ECS service to zero instances without dealing with a cold start, so you’re typically going to be paying for a minimum of one active instance at all times

This felt like a good opportunity to try to quantify the difference, so I made a quick graph that shows how much you pay for each option as the level of traffic your service receives increases:

![graph showing the cost of AWS App Runner and AWS ECS based on the number of active hours per month. The price of App Runner starts at around 5 dollars and increases linearly as the number of hours increase. The price of ECS is a flat 50 dollars per month. The two lines intersect at about 650 active hours](/app-runner-ecs.png)

If you’re using a single provisioned instance with 1 vCPU and 2GB of memory, **App Runner is basically always cheaper than ECS.** Even if you were to scale your ECS service to zero instances when you’re not using it, app runner still comes out ahead at low levels of traffic because you’re not paying for an ALB. 

In conclusion: I’m definitely going to start using App Runner for personal projects, although I’m a little more ambivalent about its advantages for a more professional operation. You lose a lot of control of load balancer configuration and task settings, and if you’re running more instances with more vCPUs and a higher baseline of activity the ECS price premium starts to go away.

With that said, App Runner definitely isn’t the _cheapest_ option out there for cloud compute. Specifically for containers, [Google Cloud Run](https://cloud.google.com/run#section-13) is less money at least for low-traffic services, and it’s hard to compete with zero dollars for a t3.micro in EC2. But as a container person and an AWS person it’s probably the option I’m sticking with for the conceivable future.
