---
title: "My time with UKESF"
date: 2023-08-22
slug: "UKESF"
description: "A reflection and post to inspire young people to get involved with electronics"
keywords: ["reflection", "electronics", "google", "domain", "warwick", "university", "student", "UKESF"]
draft: false
tags: ["ramble"]
math: false
toc: true
---

## Motivation

[UKESF](https://www.ukesf.org/) has given me the opportunity to undergo an excellent internship, as well as providing support with a plethora of resources. As such I wish to share my experience in the hopes that it will encourage fellow students to look into it. Moreover, for people that have not started university yet, I want to show a cool avenue that studying electronics can take you down.

## How it started

My journey with UKESF started with an outreach e-mail from my engineering department which introduced the scheme and directed me towards the application process. I did not have any internship plans lined up for the summer, so I was instantly curious about the opportunity and started looking through the options available.

There was a really wide variety of offers available which was positive to see as I did not feel limited in my options.

## Placement vs internship

UKESF offered both summer internships and year long placements, so it is important you consider this while applying. In my experience, I had a placement offer external to UKESF with National Grid, but I still had to choose between the two as there was a timing conflict. I am going to talk about the way in which I reached my decision in the hopes that you will be able to make a more informed decision.

### Placement

Placements offer a longer stay with your company, typically just under a year. As such you take a year out of university to complete the placement. This can be a positive or negative thing and it entirely depends on your circumstances. My position was that I had to sort out accommodation before I knew whether I had got the placement or not. Now for me this was not a huge deal since the placement would have been close enough to where I was staying, but for some people they will not get that luxury so that is something to keep in mind. Then once I knew I had it, I thought about whether I wanted to leave my year behind, specifically my housemates. Me and my current housemates were assigned to the same on campus accommodation in the first year, and so we have known each other a long time and so I was not a huge fan of leaving them for a year since by the time I got back the large majority of them would have already graduated. Moreover, I also did not like how I would leave university and get accustomed to working before having to return to university. 

### Internship

Internships offer a shorter stay over the summer, and so it does not disrupt the standard university flow at all. To me this felt better and so my search for placements on UKESF resorted to only looking for summer internships. An important note is that just because an internship is shorter, it does not mean it is less valuable or that you might not learn as much. You will find that most work places seem to work on a slower schedule than you might be used to at university. This is because work with industry typically involves many more people and all the communication that this creates significantly stretches out the progress that various teams make. However, with an internship, you can still keep a relatively fast pace since you will be less likely to have to be integrating with the entire team since the work you complete will probably only concern a small portion of the team.

## My placement

I worked at [Specialised Imaging](https://www.specialised-imaging.com/) in Cambridge for 7 weeks. This company specialises in... imaging. The company works with high-speed cameras, capturing millions of frames of a scene per second. The purpose of seeing things happen in slow motion is that you get to identify what exactly happens during a given process. For example, a lot of work relates to industry and science, monitoring split-second events in a machine or through a microscope.

More specifically, I worked at the smaller branch called Specialised Imaging Sensors, and so they are responsible for the camera sensors themselves rather than the entire chassis for the camera and other external components. My work was mostly concerned with the digital development of these sensors, so that included a lot of programming in a language called [Verilog](https://en.wikipedia.org/wiki/Verilog). I have spoken about Verilog a little in my [second-year university review](https://www.ryanbradshaw.dev/blog/second-review/#es2e3-cse-specific), so feel free to check that out.

The goal of my work was to create test patterns which are used to ensure that the codebase is working as expected when changed. For example, if the sensor chip code we are working on is changed for whatever reason, we can then re-run our test pattern to check everything is as expected. The test patterns themselves follow strict industry standards, making them applicable across companies.

Below is a particularly interesting pattern I was tasked with making. This involved generating a pseudorandom sequence with an [LFSR](https://www.sciencedirect.com/topics/mathematics/linear-feedback-shift-register#:~:text=A%20linear%20feedback%20shift%20register,its%20previous%20states%20(taps).
). This brought its fair number of challenges with it due to the stringent rules needed to conform to the standards.

![Pseudorandom](https://i.postimg.cc/GpVHFRtj/pseudorandom.png)

## My advice

There are plenty of resources outlining how to best make use of your internship or placement, so I will not include those general tips here. Instead I will be more specific, and I will also reflect on my own performance.

The first week is an important phase of the time you will spend with your company for a few reasons. If you typically find yourself as more of an introvert like myself, this will be a particularly challenging time as there will likely be many conversations to be had with your new colleagues. It is important to show up for yourself and really try to engage in these conversations, even if you would feel more comfortable sat in the corner instead. Some of the conversation will stray from work life too, and so it is important to recognise when you can be more informal and personal to build up these new relationships right from the start. As your time with the company progresses, it will get harder and harder to open up if you did not do it within the first few days since you will get accustomed to not getting involved and it will feel like a larger hurdle to ignite the conversation. At the end of the day, regardless of a person's position or age, they are still just a person. It is important to remember that so that you can engage in conversation more fluidly.

When it comes to work, it is valuable to ensure you understand the task properly before you start. It is okay to say if you do not fully understand what is being asked of you, because in the long run it will save so much time and you will be much more productive. If you manager proceeds to give you a huge list of tasks, I would also suggest asking about the priorities of tasks so that you know what to focus on. Moreover, it is important to "manage you manager" so that you do not end up with an impossible workload that they expect you to complete. Instead, if you recognise you might have too large of a workload you should state this earlier on so that the manager understands your position and limits. In my experience I would ask how long certain tasks were expected to take and then communicate my initial plan on how I would intend to go about the task. 