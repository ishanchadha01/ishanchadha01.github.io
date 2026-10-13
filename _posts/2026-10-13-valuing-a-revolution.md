---
layout: post
title: "Valuing a Revolution"
date: 2026-10-13 00:00:00-0700
related_posts: false
---

### effective altruism

Effective altruists <a id="ref-1" href="#note-1" style="scroll-margin-top: 6rem;">[1]</a> have long sounded the alarm on the dangers of artificial super-intelligence. Their mission seems noble - to quantitatively reason about which problems are the most pressing and the best strategy for tackling them. Based on their models, they speculate there will come a point of no return - a world where humans are completely reliant on AI, where AI drives its own research and advancement. But does their math check out <a id="ref-2" href="#note-2" style="scroll-margin-top: 6rem;">[2]</a>, <a id="ref-3" href="#note-3" style="scroll-margin-top: 6rem;">[3]</a>?

In the past couple years, the EA community has been plagued with poor public perception, notably surrounding their emergence as the "cult" running frontier AI labs, as well as the financial crimes of former advocate Sam Bankman-Fried <a id="ref-4" href="#note-4" style="scroll-margin-top: 6rem;">[4]</a>, <a id="ref-5" href="#note-5" style="scroll-margin-top: 6rem;">[5]</a>. As with any ideological group, there will be strong adherents, possibly to an extreme, as well as the average follower. Also, importantly, they *were right* - AI risk is very real! This is an extremely good reason to take their modeling methodology seriously.

I'm also personally invested in EA since I'm aligned with them on the front of AI safety, so I met up with a buddy on the inside and investigated some my qualms with the philosophy. Philosophy Tube has a great video on the common criticisms of EA <a id="ref-6" href="#note-6" style="scroll-margin-top: 6rem;">[6]</a>, but I focused on one particular point - ***Does optimizing for the "most good" bias their selection of problems and solutions toward only the most measurable?***

### the yardstick

EA preaches quantification of actions in terms of the amount of "good" that ensues, proposed through probabilistic measurement systems <a id="ref-7" href="#note-7" style="scroll-margin-top: 6rem;">[7]</a>.

Let's characterize this with the simple metric of QALYs and DALYs - Quality-Adjusted Life Years, and Disability-Adjusted Life Years. Essentially, QALYs are "years of life gained" and DALYs are "years of life lost":

QALY = $\sum$(quality of life improvement) $\times$ (how long is your life improved for)

DALY = YLL + YLD
where YLL = years of life lost from early death
and YLD = (years of life disabled) $\times$ (disability weight)

Kind of makes sense, but again, this is a simple example. A recent scheme that I've come across is closer to assigning a "moral weight", computing welfare rather than health:

$V(I) = \sum_{j}E[\Delta O_j \mid I]w_j$

where the value $V$ of the intervention $I$ is computed by examining all outcomes $j$. Note the two components - $E[\Delta O_j \mid I]$ is what the outcome caused, while $w_j$ is how we morally weight it.

The first term actually makes a lot of sense for short horizon events in my opinion -  take malaria nets. We can factor in the number of people reached, baseline malaria risk and reduced malaria risk based on epidemiological statistics about the usage of nets, and actual usage percentage given deployment.

The moral weight of the outcome is more suspicious, but we can still work with it. For example, we can perform Discrete Choice Experiments, which ask questions like "Would you rather receive $50 in cash, or receive an intervention that reduced your child's malaria risk by $x$?" repeatedly, varying the cash value and $x$ across numerous iterations <a id="ref-8" href="#note-8" style="scroll-margin-top: 6rem;">[8]</a>. Given a large enough sample size, this should approximately converge to true value of one unit of intervention <a id="ref-9" href="#note-9" style="scroll-margin-top: 6rem;">[9]</a>.

Before I get too far into the weeds of their modeling approach, I return to my original question - does optimizing for the "most good" lead to selection bias around easily measurable tasks? I asked my EA friend, "How do you know less easily-measurable actions, like influencing politics <a id="ref-10" href="#note-10" style="scroll-margin-top: 6rem;">[10]</a>, won't result in more good?" And his answer, to my disappointment, was that he doesn't see an alternative. He challenged me - "If you think it's more important, then just measure it"

### measure it

Now, I will (goofily) attempt to measure the value of the original tool for enacting change upon those in power: revolution. I have two reasons for picking this - one is that this is a great adversarial argument for EA's measurement system since it has several downstream effects,  long-tail timelines, and sparse experimental evidence. The second reason, as I will go on to explain, is its relevance to the power dynamics inherent to the rise of super-intelligent AI.

Generally, I believe there is merit to capitalism's efficiency; markets reveal lots of information, and information results in more certainty about the value of a good, service, or action that one can take.

However, I am also an advocate of the attributing value to those that produced it, and this is not inherent in raw, unfettered capitalism; workers are not compensated the value that they have produced, while the capitalist extracts much of the wealth <a id="ref-11" href="#note-11" style="scroll-margin-top: 6rem;">[11]</a>, <a id="ref-12a" href="#note-12" style="scroll-margin-top: 6rem;">[12]</a>. This is already exacerbated by AI - models are trained at internet-scale, and compress the data of all who have ever lived into their latent representations, without compensation or even permission.

This is partially due to the intrinsic incentives of capitalism - once innovations stop flowing, then in a poorly regulated system, the capitalist cuts corners in order to make the business more efficient. An obvious path to protect your own profit is just to reduce the wages of your workers. But independently of capitalism's nature, AI can still make this untenably bad. Imagine - the capitalist class creates a means of extracting value without the need for a working class; what's left is a moat between the masses and the elite.

So is the solution here really to throw our resources at AI safety? I truly believe that some of the best advice for a Machine Learning or Software Engineer is to devote their attention towards aligning the behavior of LLMs to be "ethical", writing robust model evaluation code, etc. But for the vast majority of Americans, there is a world where AI does not result in a utopian outcome; perhaps, the best path forward is to stop AI in its tracks. And what better way than to stop funneling our value as workers into the AI slop machine than to *revolt*?

So let's play the EA game - let's value how "good" [allocating resources towards] a worker revolution would be.

### valuing a revolution

The question I'm truly answering here is: How much "welfare" does $1 million of investment towards the chosen intervention produce, where the intervention is worker organization. I want to model the difference of welfare between whether the intervention happened or not:

$$V = \mathbb E[\sum^{T}_{t=0}\frac{ W_t(\text{additional funding})-W_t(\text{no additional funding}) }{(1+r)^t}]$$
I've added an annual discount rate $r$ too (thanks high school Econ).

For worker organization, I want to separate compensation gains from changes in political power:

$$V_{\mathrm{organizing}} = \underbrace{\mathbb E[N_{\mathrm{additional}}\;L\;\Delta u]}_{\text{direct welfare}} + \underbrace{\sum_s \left[P_s(\text{grant})-P_s(\text{no grant})\right]B_s}_{\text{political welfare}}$$
For the direct welfare term:
- $N_{\mathrm{additional}}$ are the new workers because of the grant
- $L$ is the effective number of years of the benefit
- $\Delta u$ is the annual welfare change per additional worker that's covered by the benefits
For the political term:
- the $s$ in the probability $P_s$ is a possible political trajectory, like lasting reform, transformation towards more democracy/authority, etc
- $B_s$ is the welfare from the new trajectory

Here was how I computed each term:
$$N_{\mathrm{additional}}=\frac{aG}{\tau c} \quad\Longrightarrow\quad V_{\mathrm{direct}} = \mathbb E\!\left[ \frac{aG}{\tau c}\,L\,\Delta u \right]$$
Here, $G$ is the grant, $a$ is the fraction that makes it to actual organizing expenditure, $c$ is the cost per additional member, and $\tau$ is an adjustment of the cost to the recipient's circumstances. Voos estimated costs equating to around $2,208-$5,969 in 2024 dollars, which means that $1 million would support 168-453 members at those historical productivities <a id="ref-12b" href="#note-12" style="scroll-margin-top: 6rem;">[12]</a>, <a id="ref-13a" href="#note-13" style="scroll-margin-top: 6rem;">[13]</a>.

Since $L$ is how long the welfare applies for, I made it include the discounting:
$$L = \sum_{t=1}^{T} \frac{q_t}{(1+r)^t} \quad\Longrightarrow\quad V_{\mathrm{direct}} = \mathbb E\!\left[ N_{\mathrm{additional}} \sum_{t=1}^{T} \frac{q_t\,\Delta u_t}{(1+r)^t} \right]$$
For the income component, I use log-consumption welfare - a person's annual gain is $log(c_{\mathrm{after}} / c_{\mathrm{before}})$, so doubling consumption for one year contributes $log2=0.693$ units:
$$\Delta u = \log\!\left( \frac{c(\text{grant})}{c(\text{no grant})} \right)$$

The political welfare term is a bit freaky since it also changes with scale, duration, and consequences. Instead of modeling it like:
$$\Delta P_s = P_s(\text{grant})-P_s(\text{no grant}) \qquad V_{\mathrm{political}}=\sum_s\Delta P_s B_s$$
I actually modeled variable intra-trajectory welfare too:
$$V_{\mathrm{political}} = \sum_s \left[ P_s(\text{grant})B_s(\text{grant}) - P_s(\text{no grant})B_s(\text{no grant}) \right]$$
This just leaves the $N_{\mathrm{additional}}\rightarrow\Delta P_s$ relationship - how does membership affect the probability of a consequential political trajectory? New workers aren't just units that build up toward a political outcome, and theres lots of intermediate effects that are difficult to account for. Take, for example, whether the organization does coalition building and the political stances of the collaborating orgs. For this reason, I created an intermediate variable $C$ to represent organization capacity. Essentially, I'm trying to separate how much capacity a grant creates and how capacity impacts political outcomes:
$$G \longrightarrow N_{\mathrm{additional}} \longrightarrow C \longrightarrow P_s$$
One way to express this is:
$$P_s(G) = \int \underbrace{P(s\mid C=c,X)}_{\text{political outcome given capacity}} \qquad \underbrace{f(c\mid G,X)}_{\text{capacity created under funding}} \,dc$$

where $X$ describes the political and economic context. For the $C\rightarrow P$ relationship, or the question of political outcomes given membership, there is actually some data - based on a reanalysis. I did of some international campaign data, national union participation was correlated with a 34.8 percentage points higher probability of democracy 5 years later <a id="ref-13b" href="#note-13" style="scroll-margin-top: 6rem;">[13]</a>:

$$\widehat{\theta}_{\mathrm{association}}=0.348, \qquad 95\%\text{ confidence interval}=[0.117,\;0.559]$$
To connect this with funding, let's use a narrower model where the grant only changes whether national unions participate in a consequential campaign. Let $q$ be the probability that this campaign happens, and $\delta_U(G)$ be the grand-induced increased probability of national union participation. Then:
$$\Delta P_{\mathrm{democracy}} = q\,\delta_U(G)\,\theta_{\mathrm{target}}$$
Note, $\theta_{\mathrm{target}}$ is the actual causal association, and $\widehat{\theta}_{\mathrm{association}}$ is just our benchmark of it based on the historical data.

The intermediate organization capacity model makes the remaining funding relationship explicit; if $h(c, X)$ is the probability of national union participation given capacity $c$ and context $X$, then:
$$\delta_U(G) = \int h(c,X) \left[f(c\mid G,X)-f(c\mid0,X)\right]dc$$

For the outcome's value, I used Acemoglu and colleagues' estimated democratization-GDP trajectory, where the data in question implies a 25 year log GDP effect of ~0.169 with standard error 0.0530 <a id="ref-14" href="#note-14" style="scroll-margin-top: 6rem;">[14]</a>. Summing reconstructed annual trajectory over 25 years at a 3% discount rate gives:
$$K=\sum_{t=1}^{25}\frac{\Delta\log Y_t}{1.03^t} \approx1.664$$
So far, the calculation values a political outcome; it doesn't establish how much the grant actually changes its probability. Under the assumption that GDP gains translate proportionally to consumption gains, the restricted democratization channel $G \longrightarrow N_{\mathrm{additional}} \longrightarrow C \longrightarrow P_s$ is:
$$V_{\mathrm{organizing}}-V_{\mathrm{AI}} \approx D+1.664H\,\Delta P_{\mathrm{democracy}}-A$$
I've added $A$, the welfare associated with an AI safety intervention, as an example. $D$ is the net direct welfare from the organizing grant and $H$ is the total number of people affected by the modeled democratization. So we arrive at:
$$\boxed{ \Delta P_{\mathrm{democracy}}^{*} = \frac{A-D}{1.664H} }$$
Or - how much would the organizing grant need to increase the probability of democratization to match the AI safety grant, in terms of welfare? I see two interesting cases here:
- $\Delta P^* < 0$: organizing's direct benefits already exceed AI's estimated value
- $0<\Delta P^* < 1-p_0$: the required probability increase of whether the population transitions to democracy due to the grant is possible mathematically
For illustration, suppose $A=20,000$, $D=5,000$, and $H=1,000,000$. Then:
$$\Delta P_{\mathrm{democracy}}^{*} = \frac{20{,}000-5{,}000}{1.664\times1{,}000{,}000} = 0.009014 \approx \boxed{0.90\text{ percentage points}}$$
So AI Safety starts with a 15,000 welfare point advantage, but the worker organization would move the probability of democratization from 10% to 10.9% here.

### ineffective altruism

Well, that was a fun exercise, but it points at some of the limitations of EA - data for a lot of this work was few and far between, struggling with either recency bias or historical effects not mapping to a modern context. This does make me wonder how difficult it must've been to perform this computation for AI trajectories, and how we arrived in this situation where AI is as powerful as predicted. Perhaps there were many predicted trajectories, some correct and far more incorrect, and there exists a selection bias of the issues that actually occurred...

The truth is that AI can do an immense amount of good, yet result in an immense amount of bad. Part of the "bad" is that the current American economic and political systems will leave the workers whose effort surmounted in this God technology in the ashes, as the capitalists in control watch from far beyond the flames. And even with the power of math, it remains unclear how we will escape this hell.

<section class="blog-footnotes" markdown="1" aria-labelledby="footnotes-heading">

<h2 id="footnotes-heading">notes &amp; references</h2>

<a id="note-1" href="#ref-1" style="scroll-margin-top: 6rem;">[1]</a>  aka the Illuminati of AI

<a id="note-2" href="#ref-2" style="scroll-margin-top: 6rem;">[2]</a>  fuck no

<a id="note-3" href="#ref-3" style="scroll-margin-top: 6rem;">[3]</a>  sorry

<a id="note-4" href="#ref-4" style="scroll-margin-top: 6rem;">[4]</a>  Bill Gates type shit :ew:

<a id="note-5" href="#ref-5" style="scroll-margin-top: 6rem;">[5]</a>  [crypto](https://www.jpmorganchase.com/institute/all-topics/household-financial-health/dynamics-demographics-us-household-crypto-asset-cryptocurrency-use) and other get-rich quick schemes (think, [the lottery](https://www.nber.org/papers/w28975)) tend to serve as a regressive tax. maybe SBF is fucking stupid

<a id="note-6" href="#ref-6" style="scroll-margin-top: 6rem;">[6]</a>   [The Rich Have Their Own Ethics: Effective Altruism & the Crypto Crash](https://www.youtube.com/watch?v=Lm0vHQYKI-Y)

<a id="note-7" href="#ref-7" style="scroll-margin-top: 6rem;">[7]</a>  *Doing Good Better* by William Macaskell does a pretty good job of motivating this

<a id="note-8" href="#ref-8" style="scroll-margin-top: 6rem;">[8]</a>   [GiveWell study](https://www.idinsight.org/publication/measuring-peoples-preferences) that performed a similar experiment

<a id="note-9" href="#ref-9" style="scroll-margin-top: 6rem;">[9]</a>  An interesting strategy here is that we can assign an error bound and simulate within a range of reasonable $w_j$ values as opposed to treating values from experiments as ground truth.

<a id="note-10" href="#ref-10" style="scroll-margin-top: 6rem;">[10]</a>  or making everyone hate you less

<a id="note-11" href="#ref-11" style="scroll-margin-top: 6rem;">[11]</a>  top 10 bum socialism explanations

<a id="note-12" href="#ref-12a" style="scroll-margin-top: 6rem;">[12]</a>  [Voos discussing union organization costs](https://fraser.stlouisfed.org/files/docs/publications/bls_mlr/bls_mlr_198406.pdf) and [BLS inflation data](https://www.bls.gov/regions/mid-atlantic/data/consumerpriceindexhistorical_us_table.htm). Voos estimated marginal organizing costs using data from 25 unions during 1964-77, adjusting for first-contract failures and workers not joining. I converted the reported $580-$1,568 to $2,208-$5,969 using the CPI for inflation.

<a id="note-13" href="#ref-13a" style="scroll-margin-top: 6rem;">[13]</a>  [my goat Charles Butcher](https://charlesbutcher.weebly.com/data.html). Using Butcher's replication data, I estimated the association between national union participation and democracy 5 years later after campaign end using 95 campaigns and 57 countries, controlling for baseline democracy level, log GDP, urbanization, and campaign start year. To be clear I did my own reanalysis to figure out the estimated association, it wasn't explicitly listed in the paper.

<a id="note-14" href="#ref-14" style="scroll-margin-top: 6rem;">[14]</a>  [Acemoglu and friends talking about democracy resulting in GDP growth](https://economics.mit.edu/sites/default/files/publications/Democracy%20Does%20Cause%20Growth.pdf). I figured out the annual log GDP per capita effects from the model in Table 2, Column 3, and discounted the 25 year trajectory at 3%.

</section>
