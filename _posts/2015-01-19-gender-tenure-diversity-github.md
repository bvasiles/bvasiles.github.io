---
layout: post
title: Gender and tenure diversity in GitHub teams
comments: True
---

Diversity in teams arises from any attribute that people use to differentiate
themselves from others. 
The most obvious attributes are arguably demographic (age, gender, culture, 
ethnicity), but they can also be related to pretty much everything else (for 
example, role, tenure, expertise, or even personality). 
In general, diversity in teams is viewed as a double-edged sword.
On the one hand, increased team diversity results in more varied backgrounds 
and ideas, which provide the team with access to broader information, enhanced 
creativity, adaptability, and problem solving skills.
On the other hand, due to greater perceived differences in values, norms, and 
communication styles in more diverse teams, members become more likely to 
engage in stereotyping, cliquishness, and conflict.

Diversity in teams has been studied for a long time in offline groups, but 
different studies still disagree on the effects.
Instead, we focused on distributed (online) software teams, such as those in 
Open Source Software (OSS).
OSS teams are much more fluid, therefore much less tangible, than their 
offline counterparts.
In OSS teams are *naturally* very diverse, consisting of contributors from all 
over the world, typically a mixture of volunteers and professionals, coming 
from varied cultural and educational backgrounds, with different 
interests and skills.

We focused on two diversity attributes that are prominent in OSS: **gender**
and **tenure** (experience).
Women are underrepresented in programming, and especially so in OSS.
Moreover, the "hacker" culture is said to be male-dominated and unfriendly 
to women, with reports of active discrimination and sexism.
OSS teams are inherently diverse with respect to experience, since they often 
rely on a steady influx of new contributors.
 
Then, we carefully extracted data from more than 23,000 active collaborative 
projects on [GitHub](http://github.com), the largest and most popular online 
collaborative coding platform. 
Each observation in this data set contains the composition, characteristics, 
and outcomes of a project's team of contributors for each quarter (90-day 
period) in the evolution of the project. 
Using regression analysis on this data, we modeled:

- *productivity* (the number of commits by team developers recorded in either 
the main repository or any of its forks in a given quarter), and
- *turnover* (the fraction of the team in a given quarter that is different 
with respect to previous quarter)

as functions of *gender diversity* (measured using the Blau diversity index) 
and *tenure diversity* (measured using the coefficient of variation).
We controlled for many confounds:

- *team size* (larger teams are likely associated with increased productivity 
and increased turnover);
- *overall project activity* (total number of commits);
- *project forks* (activity in forks is more likely to be limited, both in 
time and in amount, compared to a project's main repository);
- *time* (has a moderating influence on the effects of 
diversity: as￼
group members continue to interact, they have more time to 
adjust to differences between them);
- *project age* (projects that started later and their teams may have 
experienced a different GitHub culture);
- *tenure median* (to distinguish between more and less experienced teams,
regardless of how diverse these teams are with respect to tenure);
- *comments* (number of GitHub comments during a given quarter on commits, 
pull requests, and issues, as a reflection of a project's social activity).

Our models show that both **gender and tenure diversity are positive and 
significant predictors of productivity**, together explaining a small but 
significant fraction of the data variability. 

The paper presenting these results (co-authored by 
[Bogdan Vasilescu](http://bvasiles.github.io),
[Daryl Posnett](http://scholar.google.com/citations?user=IT0VNZkAAAAJ&hl=en),
[Baishakhi Ray](http://baishakhir.github.io),
[Mark van den Brand](http://www.win.tue.nl/~mvdbrand/),
[Alexander Serebrenik](http://www.win.tue.nl/~aserebre), 
[Prem Devanbu](http://www.cs.ucdavis.edu/~devanbu/),
and [Vladimir Filkov](http://www.cs.ucdavis.edu/~filkov/)) has been accepted 
for presentation at the [2015 ACM CHI Conference on Human Factors 
in Computing Systems](http://chi2015.acm.org), in Seoul, South Korea, in 
April 2015. 
A preprint containing more details is available 
[here](/papers/chi15.pdf).

This is the first academic study to consider effects of **gender** diversity on 
productivity and turnover in OSS communities. 
On a larger, economic and societal scale, these findings suggest that added 
investments in educational and professional training efforts and outreach for 
female programmers will likely result in added overall value. 

