---
layout: post
title: Neural Rendering and Surveillance
date: 2026-09-20 00:00:00-0700
related_posts: false
---

I used to think surveillance is deeply unpopular, but after moving to Silicon Valley, I was surprised by the tone with which my left-leaning peers viewed the issue (*hint* - but how will we catch the bad guys??). On the other hand, my even-lefter-leaning peers have been calling out the privacy risks associated with the deployment of self-driving cars, which I also would not have foreseen.

I guess sometimes it takes a bout of shower thoughts or an introspective time on the toilet to connect the dots [1], so here I've gathered my opinions on the current state of surveillance technology. In doing so, I'm considerably more concerned than prior to writing this essay, and I feel that we are in more desperate need of robust AI policy than ever.

Broadly, surveillance technology has always bothered me because of how it's historically been employed to paint a narrative against groups that I care about [2]. Since 2021, the conversation has shifted though - we're not talking about Cambridge Analytica and Russian operations influencing US elections, nor are we talking about the policing of Black neighborhoods in the context of BLM. Several of us now see the issue via the lens of immigrant distrust, and for some silly reason, others are misled by the seemingly democratic nature of both the US government and technology...

Let's start with the perv glasses! Many have purchased Meta's Ray Bans, which have the diddy camera [3] hidden next to the lens, and amongst some, this has deemed them "perv glasses" [4] -- rightfully so, since it's easier to record people in a hidden manner than ever before.  

\**nightmare mode*\* But did you know that data from these glasses can be reconstructed into a high fidelity 3D model insanely quickly? Did you know you can render never-before-seen views of the subject (*cough* victim)??

Ok maybe it's not that crazy, everyone has seen how drastically generative AI has improved. But irrespective of your opinion on the US government and its surveillance tendencies over the years, reconstruction-enabled tech is proliferating to governments that have explicit histories of centralized surveillance. Waymo cars (all enabled with World Modelling) are only required to give up information to law enforcement when administered a valid legal request, such as a recent incident where a firearm was detected inside a Waymo [5]. If you're not already hesitant about the US government and its tendencies towards surveillance of out-groups [6], Wayve just signed a deal with the UK, and Uber and WeRide have been deployed in Saudi Arabia. All of this to say, powerful surveillance-enabled tech is floating around while its backers are incentivized to deny issues [7].

But then there's the tier above that - that's right, I'm talking Palantir and Flock Safety. The history of Palantir is inherently political and its investors, board, and founders have complex motives and goals that would take a while to disentangle; maybe another time. Flock safety, however, is the chud version of Palantir, which is why the open source community was able to publicly reject and clown them by reversing their craft so the public could avoid its camera fleet [8]. What's that? The open source community has always been a spiteful group of libertarians who are chuds themselves [9]? Well, let me point you to how the consumers are using this product in our government! Surely our police force would not have the ability to use such a system to stalk their ex girlfriends... surely [10].

There are two fairly important questions that I haven't answered:
- We've talked about the state of modern surveillance, but why does 3D reconstruction enabled tech matter here?
- Surveillance tech is already ubiquitous, the dynamics point towards up-scaling, and the first-order benefits seem reasonable (less crime, better truth-seeking, AI that can reason about the physical world), so *should* we care? [11]

> why does reconstruction enabled tech matter here?

Heads up, I'm going to get pretty technical in this explanation. My claim here is essentially that surveillance is transforming from recording observations to vast, networked, intelligent devices creating manipulable models of the world, and only tech giants will have access to this moving forward.

In 2023, I was halfway through my Master's thesis on 3D reconstruction in medical settings, and a single paper completely changed its course: [3D Gaussian Splatting for Real-Time Radiance Field Rendering](https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/). This paper enabled *significantly* faster and more accurate 3D reconstruction, but for large scenes with many dynamic actors, the process remained slow for high fidelity reconstructions. Regardless, numerous companies and products spun out of this, such as World Labs and Google Deepmind's Genie 3.

Previously, a method called NeRFs were state-of-the-art for 3D reconstruction, but they involved training a humongous neural net:

(x, y, z, $$\phi$$, $$\theta$$) -> (RGB, $$\sigma$$)

So we could ask the question, given our 3D position *xyz* and our viewing angle ($$\phi, \theta$$), what RGB color and opacity $$\sigma$$ would we see? 

{% include figure.liquid path="assets/img/neural-rendering-and-surveillance/figure-1.png" class="img-fluid rounded" %}

This involved querying the neural net many times along the ray and accumulating color until it saturated, which was very computationally expensive and time-consuming for a single scene. Imagine training this - you'd have to collect enough training data to generalize to multiple viewing directions for every point in 3D space.

But then 3D Gaussian Splatting came to be; instead of trying to capture data for every viewing pose, we just needed to develop 3D representations for all the objects in the scene. If we know where's a tree in the scene and we've seen it from a couple spots, then model the tree with some building blocks (in this case, 3D Gaussians), and then project that onto your canvas ("splatting", or projecting, the 3D Gaussians onto your 2D camera space).

{% include figure.liquid path="assets/img/neural-rendering-and-surveillance/figure-2.png" class="img-fluid rounded" %}
*we're basically modeling the scene as a jar of jelly beans ngl*

Each Gaussian in 3DGS is represented by an *xyz* point in space, a rotation matrix R, a scaling along each axis $$s_x, s_y, s_z$$ , and spherical harmonics [] which encode how the color changes with respect to viewing direction. Although 3DGS has to store a bunch of information for each building block in the scene, actually projecting this from 3D to 2D is a very quick process with some GPU chicanery, which sped up 3D reconstruction to unprecedented levels.

{% include figure.liquid path="assets/img/neural-rendering-and-surveillance/figure-3.png" class="img-fluid rounded" %}
*Data from the linked original 3DGS paper*

6 minutes! versus 48 hours for Mip-NeRF! And we're not even done yet. There were some important updates that continued to speed this up and improve accuracy and generalization:

[**3DGUT**](https://arxiv.org/abs/2412.12507) - The process of "3D Gaussians" -> "project to 2D eliipses" -> "determine footprint on pixels" is known as Elliptical Weighted Averaging, required computing the Jacobian of a nonlinear projection, resulting in approximation errors for Gaussians that are very long/narrow or subpixel in screen footprint as well as non-pinhole cameras, so 3DGUT uses unscented transforms (same sampling method as Unscented Kalman Filters) to estimate this nonlinear transform with only 7 points. One consequential outcome of this was the ability to model secondary rays, which means we could now model the physical properties of an object in the scene as well (think "secondary ray" = "the ray after light bounces off the object").

[**MCMC**](https://arxiv.org/abs/2404.09591) - Gaussians are modeled as a random sample drawn from the scene's underlying "probability distribution", which gets rid of the carefully engineered Gaussian pruning/splitting that needed to occur to move Gaussians around in the scene when areas were too sparsely/densely modeled.

And finally, in the past couple months, feedforward methods have made 3D reconstruction lightning fast

***feedforward*** - We're now in the realm that we don't need scene optimization at all. This is what it sounds like - large vision models (same building blocks as LLMs for the most part) learn a general reconstruction function on millions of views instead of learning per-scene geometry that was required with NeRF and 3DGS training schemes.

With foundation models, scaling laws govern. Those who hold massive data and compute - frontier labs - have already instated their control over 3D reconstruction, and nobody is talking about it yet. 

{% include figure.liquid path="assets/img/neural-rendering-and-surveillance/figure-4.png" class="img-fluid rounded" width="697" %}
From [VGGT-$$\Omega$$ paper,](https://vggt-omega.github.io/) a clear correlation between model size and model performance

I'll be real - I've long thought that as LLMs grow their footprint in society, we will continually lose the contract of trust between humans and digital truth. We've seen this with misinformation, and countermeasures have been based in both technical development and policy. But the leaps in scale that I'm seeing of information corruptibility, combined with networked deployments of intelligent sensing systems, make me reluctant to say that we will be able to naturally pace this [12]. And this brings me to the second question:

> *should* we care? will solving encompassing/adjacent problems suffice?

I'm just gonna say it - the safeguards that we normally envision for frontier lab LLMs don't generalize to the risks posed by ubiquitous machine perception [13]. Imagine we've solved AI alignment - our AIs don't hallucinate, they follow the operator's instructions, and they don't get hacked. How do we address

- when a police department uses it to reconstruct everyone at a protest
- one government collecting data with benevolent privacy practices and other repurposing/misusing it upon transfer
- a truthful model creating a misleading narrative based on how the operator prompts it

Somehow, the properties of truthfulness, interpretability, and adherence to operator beliefs in this scenario become a weapon? Historically, the narratives painted by surveillance were not always incorrect; sometimes they were often real observations, which were interpreted and characterized in a specific, scathing way. Ruth Hubbard said it best - "Truth is in the eye of the beholder." - Ruth Hubbard, *Science, Facts, and Feminism*, 1988.

[1] why not both simultaneously? Personally i'm a waffle stomper
[2]
- Oppenheimer and McCarthyism
- NYPD, the Browns, and 9/11
- FBI infiltration of the Black Panthers
I could go on

[3] coining this
[4]  [article](https://techcrunch.com/2026/09/16/after-accusations-of-selling-perv-glasses-meta-prepares-to-sell-a-pair-without-a-camera/)
[5]  [article](https://www.theverge.com/transportation/996863/robotaxi-waymo-police-privacy-surveillance) , also idk how people get it goin in one of these but [article](https://sfstandard.com/2023/08/11/san-francisco-robotaxi-cruise-debauchery/)
[5]  [insert article about sex in waymos] 
[6] 
- Japanese internment during WWII
- the Lavender Scare and Gay policing
- FBI investigation of MLK over "Communist ties"
should i keep going

[7] "oh no my product misbehaved" -> "oh no i've lost my investment", like Cruise ([she got run over twice](https://www.justice.gov/usao-ndca/pr/cruise-admits-submitting-false-report-influence-federal-investigation-and-agrees-pay) :sob:) or Tesla ([data from consumer-owned vehicles shared in chatrooms](https://www.reuters.com/technology/tesla-workers-shared-sensitive-images-recorded-by-customer-cars-2023-04-06/?utm_source=chatgpt.com) :bruh:)
[8]  LFG [DeFlock](https://mashable.com/tech/flock-camera-map-neighborhood) and [Zuckoff](https://www.wired.com/story/zuckoff-app-sees-meta-glasses-before-they-see-you/)
[9]  I don't even believe this myself, but ragebait is healthy for the soul
[10]  [article](https://www.cnn.com/2026/08/26/us/flock-kentucky-police-officer-arrest) man do I wish I was a cop. Insta stalking hasn't been doing it for me lately
[11] Another framing of this question is whether solving broader or adjacent problems like AI control/alignment will automatically materialize the best outcomes of surveillance tech.
[12] wording chosen intentionally, @Dario please pace my frontier :pray:
[13] Alignment going well is a very optimistic view imo. With the recent [OpenAI/HuggingFace breach](https://www.dwarkesh.com/p/openai-huggingface), I can't help but fear-monger that even if these companies try and follow through on what they're promising, the existence of this tech opens the possibility for unforeseen attacks paths with an unprecedented capability of hacking and social engineering.

