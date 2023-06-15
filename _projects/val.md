---
title: VAL
layout: post
img: /images/val.gif
date: 2023-05-24
comments: false
---

The Verbal Apprentice Learner (VAL) is a framework for acquiring hierarchical task knowledge from natural dialog. VAL's semantic parser uses a pre-trained GPT language model to perform individual, highly granular tasks, such as predicate-argument extraction, semantic matching, and argument grounding. The result is a system that can convert natural dialog into formal instructions that are grounded in the operations and objects available in a specific environment. VAL then uses classical learning techniques, like hierarchical task decomposition and argument variablization, to convert these instructions into interpretable, reusable, symbolic knowledge structures.

<video controls autoplay muted width="640">
	<source src="../videos/val-demo.webm">
</video>

As you can see in the demo video above, VAL's use of GPT makes it resistant to typos (e.g., "prsee space"), paraphrases (e.g., "walk up to" vs. "go over to" vs. "move to"), and ambiguous references (e.g., "put it in the pot"). You can also see how the "cook" action is taught in an environment with an onion, and then transferred automatically to tomato soup in the second half of the clip; this highlights the generalization procedures that make VAL a good "lifelong learner".

### Relevant Publications

Lawley, L., MacLellan, C.J. (2023). Interactive Learning of Hierarchical Tasks from Dialog with GPT. arXiv preprint arXiv:2305.10349. [(pdf)][lawley-maclellan-2023]

[lawley-maclellan-2023]: https://arxiv.org/pdf/2305.10349.pdf
