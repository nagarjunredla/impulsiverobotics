---
id: 178
title: 'A Neural Network a day'
date: '2018-07-10T18:48:07+00:00'
author: 'Nagarjun Redla'
layout: post
guid: 'https://impulsiverobotics.com/?p=178'
permalink: '/?p=178'
draft: true
categories:
    - Posts
---

If there is anything I’ve learned from Graduate School that could be useful for future Graduate Students, it’s that you should never rush it. Getting done with it early seems like a good option if you’re studying in a college town that’s white with snow for six months and scorching hot for another three, but do consider staying the entire duration of your time at Grad School. I feel it’s necessary in today’s world where a lot of fields require multi-disciplinary knowledge and experience that it’s usually difficult to graduate with a vast skill set an employer would be interested in for a fresh student with no experience (or at least it seems that way to me at the moment). If you’re in a rush you might [miss opportunities](https://www.reddit.com/r/UIUC/comments/7xjxy8/seth_hutchinson_an_ece_faculty_who_just_left_for/), [take classes that aren’t ready yet](https://www.reddit.com/r/UIUC/comments/88dmtl/should_i_take_both_cs_446_and_cs_498_aml/dwllkqs/?context=0) ([or professors during syllabus revamp](https://www.reddit.com/r/UIUC/comments/8r004g/cs_498_deep_learning_vs_cs_446/e0qlw8b/)), or simply dive deep into one topic you love (Control Theory in my case). I’m not complaining. Technically, I took the best the University had to offer, but I did it too fast.

So if you’re going to get back to looking for jobs right after, and you feel like you don’t have enough proving ground for employers, what do you do? You show off some projects. But what projects do you come up with during the 90-day-get-a-job-or-get-lost period in ML? Though it’s everywhere it is still pretty nascent at the moment. Remember how it seemed so trivial that cameras could find your face or detect a smile (before smartphones existed)? Well breakthroughs in ML that enabled such technology didn’t happen that long ago, bud. My background is in Electrical Engineering with a hint of Machine Learning. The ML training I had wasn’t on par with CS guys trained in Data Science as a whole, while at the same time the lack of everyday comfort with CNNs seemed like a gap when it came to autonomous vehicles. With some AWS credits pooled up from attending conferences and Azure being kind enough to spare some credits, I thought I’d just go for it and learn. There’s always Kaggle, but I needed some confidence in my head and some clarity before attempting a competition.

A quick reddit-Google search for “best machine learning papers” resulted in [one comment](https://www.reddit.com/r/MachineLearning/comments/8vmuet/d_what_deep_learning_papers_should_i_implement_to/e1pj0ia/) that enumerated quite a few networks, predominantly in Computer Vision. As much as I want a robot making/playing me music, I thought this would be a good place to start. This could probably let me port those networks to the Neural Compute Stick along the way, since I almost gave up trying to find a trained single network for face detection (yeah I thought it was trivial too) that worked on the NCS. Already blew a week in my countdown, this hard-deadline plan might fail miserably but I’m going to do it anyway.

A Neural Network a day. Let’s go!