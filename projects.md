---
layout: page
---

### <a name="scalingup"></a>Scaling up distributed software development

The *pull-based model* is an innovation made popular by 
[GitHub](http://github.com), to speed up and scale up distributed
development: anyone can submit *pull request* contributions 
(proposed changes) to any project. 
Pull requests are reviewed by core developers (aka integrators) 
and accepted only if deemed of sufficient quality and utility. 
This low barrier to entry for newcomers, together with GitHub’s 
scale ([30 million projects](https://github.com/about)), means 
that <font color="SlateGray">*developers have a great many projects to choose from*</font> and 
that <font color="SlateGray">*integrators have increasingly higher review loads*</font>.

#### <a name="onboarding"></a>Socialization is a precursor to developers joining a new project

<img style="float: right;" src="../images/gh-network.jpg" alt="CI" width="40%">

We have already seen in prior work that 
<font color="SlateGray">*socializing is very important for
advancement in open source software development*</font> 
([see our EMSE'14 paper](../papers/ese14.pdf)):
one ramps up their communication activity like never before 
prior to being promoted to core developer status.
But how do social connections and experience affect 
people's migrations between projects, and their technical 
contributions?

We mined data for 1,200 prolific developers from GitHub, and 
used statistical modeling to precisely delineate these factors.
As expected, we found that <font color="SlateGray">*having past 
social connections with project members is a precursor to 
developers joining a new project*</font>.
Moreover, it is associated with
<font color="SlateGray">*higher productivity*</font>.
Our models enable us to estimate precisely how 
important each factor is
([see our FSE'15 paper](../papers/fse15onboarding.pdf)):

- Joining a new project in which there are some prior social 
connections with existing members increases a developer's 
chances for contribution above baseline in the first six
months by 3.7% to 6.2%; similarly so for joining a project 
with familiar languages.
- Beyond the first six months, 
having prior experience with the languages of the project 
and prior social connections with other members gives a 
developer a great advantage in overall productivity, with 
an increase by 29.5% to 54.3% over baseline.- However, developers who join environments where they have 
prior social connections but no prior language experience 
are 9.6% less likely to have long-term productivity above 
baseline.

#### <a name="ci"></a>Continuous Integration (CI) speeds up the pull request process

<img src="../images/ci.jpg" alt="CI" width="80%">
To cope with high review loads, integrators can and do resort 
to CI services such as [Travis-CI](http://travis-ci.org)
([see our MSR'15 paper](../papers/msr15.pdf)). 
CI attempts to automatically build and deploy the software 
in a "sandbox", and automatically run a collection of tests 
when a pull request is received. 
By automating these steps, a project can hope to gain both 
productivity (more pull requests accepted) and quality (the 
accepted pull requests are prescreened by the automation 
provided by CI).
Does CI actually help to speed up the pull request process 
and to ensure higher quality code?

We mined data from 246 GitHub projects that added the 
Travis-CI functionality to their development process at 
some point in their history.
Using this data set, we built zero-inflated regression 
models to compare activity before and after adoption of
Travis-CI.
We found that:

- After CI is added, <font color="SlateGray">*20% more pull 
requests from core developers are accepted, and 26% fewer 
submissions from non-core developers get rejected*</font>. 
This suggests that CI improves the handling of insider 
pull requests, and has an overall positive effect on the 
initial quality of outsider submissions.- Despite the increased volume of pull requests accepted, 
<font color="SlateGray">*introduction of CI is not associated 
with any increase/decrease in user-reported bugs*</font>, thus 
suggesting that user-experienced quality is not negatively 
affected. 
We did see an <font color="SlateGray">*expected increase 
in the number of bug reports by core developers in projects 
that use CI of 48%*</font>, which suggests that CI is 
helping developers discover more defects.
([see our FSE'15 paper](../papers/fse15ci.pdf))

---

### <a name="diversity"></a>Diversity

Due to largely meritocratic hiring, software teams can be 
quite diverse, e.g., with respect to gender, cultural background, 
and programming language preference. 
However, women are still underrepresented in programming 
(industry reports 16-18% female developers; open source 
around 10%). 
On social coding platforms, the situation is even worse: 
in a [GitHub](http://github.com) sample, I estimated 9% 
female developers 
([see our CHI'15 paper](../papers/chi15.pdf) and 
[our MSR'15 paper](../papers/msr_data15.pdf)) and on 
[Stack Overflow](http://stackoverflow.com) only 7% 
([see our IWC'14 paper](../papers/iwc13.pdf)). 
Why are modern social coding platforms so exclusive? 
Is it related to community design? 
Is it an intrinsic characteristic of software teams? 

#### <a name="gendergh"></a>More diverse teams are more productive

<img style="float: right;" src="../images/diversity1.jpg" width="35%">
Using data from thousands of projects on GitHub, we 
analyzed the relationships between team diversity, 
productivity, and turnover using generalized linear 
mixed-effects models ([see our CHI'15 paper](../papers/chi15.pdf)). 
We found that <font color="SlateGray">*both gender 
and tenure diversity are positive and significant 
predictors of productivity*</font>: 
teams that are more balanced in terms of gender and 
seniority have higher productivity rates. 

Through a large-scale survey (800+ developers), we 
found that respondents value diversity for their team's 
creativity and problem solving effectiveness. 
Despite these many positives, a handful of female 
respondents reported frustration and disengagement 
from a project due to gender-related incidents
([see our CHASE'15 paper](../papers/chase15.pdf)).

#### <a name="genderso"></a>Social coding comes at a social cost

<img style="float: right;" src="../images/MenWorkingRoadSign.jpg" width="40%">
We compared activity on Stack Overflow, a gamified Q&A 
platform where people are rewarded with reputation 
points and badges, with that on traditional mailing 
lists. 
We found that <font color="SlateGray">*on Stack Overflow 
women disengage sooner than men, although relative to 
their tenure they are comparably active*</font>. 
We predict that the achievement-oriented nature of 
Stack Overflow, cultivated by the many gamification 
elements in its design, is creating an environment 
hostile to women, driving them away from participation.
([see our IWC'14 paper](../papers/iwc13.pdf))

---

<!--### Signaling

<img src="../images/signaling.jpg" alt="CI" width="80%">

---

### Migration to social Q&A websites 

<img src="../images/ml-se.jpg" alt="CI" width="80%">

---
-->