---
permalink: /phd/
title: "Measuring the invisible: how much force does a single player produce in a rugby scrum?"
excerpt: "Three years of PhD research with Stade Toulousain: instrumenting elite rugby players with instrumented insoles and building machine learning models to estimate individual ground reaction forces during scrummaging."
author_profile: true
read_time: true
# date: 2025-10-01   # uncomment to display a publication date under the title
# header:
#   teaser: scrum-teaser.jpg      # file placed in /images/
#   overlay_image: scrum-hero.jpg # full-width banner, file placed in /images/
#   overlay_filter: 0.4
---

<!--
================================================================================
DRAFT — recruiter-oriented version.

Everything marked **[to complete: ...]** is a placeholder: only you have those
numbers and details. They are deliberately shown in bold on the page so you
cannot forget one before going live.

Text between <!- - and - -> is a comment: invisible on the website.

Useful snippets:
  Image with caption:   ![Alt text](/images/my-figure.jpg)
                        *Figure 1. Caption.*
  Highlighted box:      Some sentence.
                        {: .notice--info}
  Video:                <iframe width="560" height="315"
                          src="https://www.youtube.com/embed/VIDEO_ID"
                          frameborder="0" allowfullscreen></iframe>
================================================================================
-->

{% include toc icon="book" title="Contents" %}

<!--

**PhD in biomechanics (2022–2025)** — Institut Clément Ader & LAAS-CNRS, in partnership with **[Stade Toulousain](https://www.stadetoulousain.fr/)**, one of the most successful rugby clubs in France and Europe.<br>
**In short:** I built a measurement chain — wearable sensors, experimental protocols and machine learning models — that estimates the three-dimensional force produced by *each individual player* during a rugby scrum, in real conditions rather than in a laboratory.
{: .notice--info}

## The context: a black box at the heart of rugby

A scrum puts sixteen players in direct contact, eight against eight, in one of the most physically demanding phases of any team sport. It decides possession, territory and, quite often, matches. It is also one of the main injury mechanisms for front-row players.

Yet the coaching staff of a professional club has almost no objective data on it. Instrumented scrum machines measure the **total** force of a pack against the machine — a single resultant, for eight players at once. Nothing tells a coach whether a given player is pushing hard, pushing in the right direction, or quietly being carried by his teammates.

That is the gap I worked on: **going from one global force to eight individual forces.**

### Why Stade Toulousain

The project was run in direct collaboration with Stade Toulousain, a club with a professional performance department and international-level front-row players. This shaped the entire thesis, in ways that turned out to be as much of a scientific constraint as an opportunity:

- The measurements had to be made on **elite athletes in their real training environment**, not on students in a laboratory. The biomechanics of a professional prop has little to do with that of a volunteer.
- Data collection had to fit inside **the club's schedule**, not the other way around: short sessions, no interference with training load, equipment that must work the first time.
- The output had to be **usable by a performance staff**, not only publishable — which forced me to think about interpretability and deployment from the start.

**[to complete: add 2–3 concrete facts here — number of players involved, number of sessions, over how many seasons, whether the staff used the results in practice. This is exactly the kind of detail a recruiter reads as evidence of real-world impact.]**

## Which measurement solutions were on the table

Estimating individual force during scrummaging is not a matter of buying the right sensor: no sensor gives that measurement directly. I reviewed and tested several routes, each with a fundamental limitation.

**Force platforms.** The reference method in biomechanics: they measure ground reaction forces in three dimensions with excellent accuracy. But they are fixed in the ground, cover a limited area, and require each foot to land on a specific plate. They cannot be taken to a training pitch, and they cannot instrument eight players simultaneously in a real scrum.

**Instrumented scrum machines.** Robust, ecological, already used by clubs — but they only give the resultant force applied to the machine. The individual contribution of each player remains inaccessible, and a machine is not an opposing pack.

**Sensors on the contact surfaces** (shoulder pads, instrumented shirts). They measure what happens at the point of contact between players, but not what the player produces against the ground, which is the actual source of the pushing force. They are also intrusive and highly sensitive to positioning.

**Instrumented insoles.** Wearable, non-intrusive, worn by every player at once, usable anywhere — including in a real scrum against a real opposing pack. This is the only option compatible with the ecological validity the project required.

**The solution adopted in the thesis: instrumented insoles**, because they are the only technology that lets you instrument every player individually, in situ, without changing the way they scrummage.

**[to complete: name the insole system you used and its key specifications — number of sensors, sampling frequency. Recruiters in sports tech and instrumentation will look for this.]**

## The catch — and why machine learning became necessary

Instrumented insoles come with a serious limitation, and confronting it is the scientific core of the thesis.

Insoles measure **plantar pressure**: a distribution of normal pressures under the foot. From this, one can reasonably reconstruct the **vertical** component of the ground reaction force. But in a scrum, the performance-relevant variable is the **horizontal** component — the anteroposterior force with which a player pushes forward. That is precisely the component insoles cannot measure. Add to this sensor drift, calibration sensitivity and the deformation of the insole inside a boot under extreme load.

So the problem became: **how do you recover a three-dimensional force from a sensor that only sees one dimension?**

The answer is not a better sensor, it is a model. The pressure pattern under a foot is not independent of the force being applied in the other two directions: how a player loads the front of the foot, how the centre of pressure shifts, how the load distributes between both feet — all of it carries information about the horizontal push. That relationship is far too complex to write down analytically, but it can be **learned from data**.

The strategy:

1. Record, in controlled conditions, **synchronised data** from instrumented insoles *and* force platforms — the insoles provide the input, the platforms provide the ground truth.
2. Train machine learning models to map insole signals onto the full 3D ground reaction force.
3. Deploy the trained models **on the pitch**, where no force platform can ever go.

The force platforms are not the measurement system: they are the teacher. Once the model is trained, they are no longer needed.
{: .notice--info}

## The machine learning work

### Building a usable dataset

**[to complete: describe the experimental campaigns — number of participants, scrummaging conditions (individual pushes, machine scrums, full-pack scrums, live scrums against an opposing pack), number of trials, total volume of data. Also mention the synchronisation of heterogeneous systems (insoles, force plates, motion capture if used), which is a genuine technical skill in itself.]**

A significant part of the work was not modelling but **data engineering**: synchronising devices that do not share a clock, segmenting scrum phases in continuous signals, handling missing sensors, and normalising across players whose body mass ranges over several tens of kilograms.

### Model selection and architecture optimisation

I compared several families of regression models rather than committing to one a priori, and optimised the architecture and hyperparameters of the most promising ones.

**[to complete: list the models you compared — e.g. linear regression as a baseline, random forests / gradient boosting, fully connected neural networks, temporal models (LSTM/CNN) — and say which one won and why. Mention the hyperparameter optimisation method you used (grid search, Bayesian optimisation...) and the input representation you settled on (raw pressure maps, engineered features, temporal windows...).]**

The baseline matters as much as the winner: showing that a simple model is not sufficient is what justifies the complexity of the final one.

### Comparing training strategies

The most interesting methodological question of the thesis was not *which algorithm*, but *how to train it* — because the intended use is to put the insoles on a **new player** who was never part of the training set.

I compared training strategies along that axis:

**[to complete: describe the strategies you compared, for example — subject-specific models vs. a single generic model; leave-one-subject-out cross-validation to measure true generalisation to unseen players; per-player fine-tuning from a generic model; normalisation by body mass; data augmentation. State clearly which strategy generalised best, and the cost/benefit for a club — a generic model works immediately on any player, a personalised model is more accurate but requires a lab session per player.]**

This is a generalisation problem, not an accuracy problem, and it is the one that determines whether the method is deployable by a club or stays in a laboratory.

### Taking it to a real scrum

**[to complete: describe the on-field application — real scrums with Stade Toulousain, what was measured, what the models produced, and any comparison against an independent reference (instrumented scrum machine total force vs. the sum of individual estimated forces is a particularly convincing validation if you did it).]**

### Main results

**[to complete: your headline numbers. For each force component (vertical, anteroposterior, mediolateral): RMSE or normalised RMSE, correlation with the reference, and how that compares to what already exists in the literature. Two or three figures, each with a one-line caption, will do more here than a page of text.]**

Published in the *Journal of Biomechanics* (2025) and presented at the 3DAHM international symposium (2024) — see the publication list below.

### What comes next

**[to complete: work in progress or under review, and the perspectives you see — real-time feedback to the coaching staff, individual load monitoring across a season, injury-risk applications, transfer of the method to other contact phases or other sports, and how this connects to your current postdoctoral work at LAAS-CNRS.]**

## What this project demonstrates

<!-- This section is written for recruiters who skim. Keep it factual and short.
     Delete any line that overstates what you actually did, and add the tools
     and languages you really used. -->

<!--

- **Sensor instrumentation and measurement chain design** — selecting, calibrating and validating wearable sensors under extreme mechanical loads.
- **Experimental design in an elite sport environment** — running protocols with professional athletes, where you get one attempt and no second session.
- **Multi-system data acquisition and signal processing** — synchronising force platforms, instrumented insoles **[to complete: and motion capture, if applicable]**, segmenting and cleaning noisy real-world signals.
- **Applied machine learning for regression** — model selection, hyperparameter optimisation, and cross-validation strategies designed for generalisation to unseen subjects rather than for optimistic test scores.
- **Turning a research result into something usable** — designing for deployment outside the laboratory, with the constraints of the end user in mind.
- **Communication** — peer-reviewed publications, international conferences, and translating technical results for a coaching staff.

**[to complete: add your technical stack — Python, scikit-learn, PyTorch/TensorFlow, pandas, etc. — and any other language or tool you worked with.]**

## Publications from this thesis

- *[Machine learning techniques for estimating the individual three-dimensional ground reaction forces during rugby scrummaging](https://hal.science/hal-05330563/)*.
**Zoé Pomarat**, Jean-Charles Passieux, John-Eric Dufour, Bruno Watier.
**Journal of Biomechanics**, 2025.

- *[Estimation of Ground Reaction Forces in Rugby Scrummaging Using Instrumented Insoles and Machine Learning](https://hal.science/hal-04925876/)*.
**Zoé Pomarat**, Kahina Chalabi, Maxime Sabbah, John-Eric Dufour, Jean-Charles Passieux, Bruno Watier.
**19th International Symposium on 3D Analysis of Human Movement (3DAHM)**, Montevideo, Uruguay, 2024.

<!-- Once you upload the files to the files/ folder, they are served at /files/name.pdf :

- [Full PhD manuscript (PDF)](/files/phd-manuscript.pdf)
- [Defence slides (PDF)](/files/phd-defence-slides.pdf)
-->

<!--
## Acknowledgements

This work was supervised by Prof. Bruno Watier, Prof. Jean-Charles Passieux and Asst. Prof. John-Eric Dufour, at the [Institut Clément Ader](https://institut-clement-ader.org/) and [LAAS-CNRS](https://www.laas.fr/en/), in collaboration with [Stade Toulousain](https://www.stadetoulousain.fr/).

**[to complete: funding body, and the club staff / players you want to thank.]**

-->
