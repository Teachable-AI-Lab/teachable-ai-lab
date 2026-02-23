---
title: Verbal Apprentice Learner (VAL)
layout: project
img: /images/val.gif
date: 2023-12-01
comments: false
order: 6
category: current
---

The Verbal Apprentice Learner (VAL) is a framework for acquiring hierarchical task knowledge from natural dialog. VAL's semantic parser uses a pre-trained GPT language model to perform individual, highly granular tasks, such as predicate-argument extraction, semantic matching, and argument grounding. The result is a system that can convert natural dialog into formal instructions that are grounded in the operations and objects available in a specific environment. VAL then uses classical learning techniques, like hierarchical task decomposition and argument variablization, to convert these instructions into interpretable, reusable, symbolic knowledge structures.

![val-demo]

As you can see in the demo video above, VAL's use of GPT makes it resistant to typos (e.g., "prsee space"), paraphrases (e.g., "walk up to" vs. "go over to" vs. "move to"), and ambiguous references (e.g., "put it in the pot"). You can also see how the "cook" action is taught in an environment with an onion, and then transferred automatically to tomato soup in the second half of the clip; this highlights the generalization procedures that make VAL a good "lifelong learner".

[val-demo]: ../images/val.gif
