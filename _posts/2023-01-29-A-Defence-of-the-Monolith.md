---
layout: post
title:  "A Defence of the Monolith"
tags: architecture monolith microservices
language: EN
---

>The Monolith should be the sensible default choice as an architectural style. 
In other words, I am looking for a reason to be convinced to use microservices, rather than looking for a reason not to use them. [^1]

That affirmation could be counter-intuitive, even more as we see a great deal of new application developments **starting from zero with a Microservices architecture**. But that is how it works a lot of times in the software industry: we take a path because everyone is taking it, because is the latest trend or because we want to be prepared in case we are the next Twitter, Google or Amazon. 

In the end we all know that a monolith is not agile: making one change means re-deploying the whole application, everything is coupled, maybe it uses a shared database (oh, my God), and it doesn't scale well (arguable).


## And we want to be agile, don't we?


First of all, applying a Monolithic architecture **does not mean that you en up contravening SOLID principles or good coding practices**. A Monolith is an arquitectural decision. You can, and must, have separation of concerns, loose coupling and so on.

Second, in the first stages of a software project a Monolithic architecture is, probably, more agile than a Microservices arquitecture, because:
- It has much simpler developer workflows
- Simplified monitoring, troubleshooting and Testing
- Simplified reuse of code ( just a shared library)

In the first stages of a project, mixing the definition of the problem domain with the specific details of how it is going to be implemented in a distributed environment can distract us and make us lose the focus.


## When to distribute our project


>A distributed system is one in which the failure of a computer you didn’t even know existed can render your own computer unusable.
—Leslie Lamport

**A distributed architecture comes with a cost.**

If your services are going to be totally independent they are going to interchange a lot of data, maybe to a streaming processing framework. You are going to have to think about these details and make decisions. And that increment in complexity should be cost-effective.

You change something when it **starts** giving you problems. Maybe you are waiting for one team to reflect the changes of another team to deploy, and feel that the architecture is limiting you. Or maybe you Monolith doesn't scale well and you have to split the problematic parts to scale them separately.

The key in the sentence above is ''**starts**'', if we wait too long the developers are going to solve that limitations with shortcuts, and by when you want to change it will be more difficult.

But, keep in mind that you could always first [modularice the Monolith](https://shopify.engineering/deconstructing-monolith-designing-software-maximizes-developer-productivity#:~:text=A%20modular%20monolith%20is%20a,enforced%20boundaries%20between%20different%20domains.), restricting the boundaries even further. Then distributing it.

---
#### References

[^1]: [Building Microservices 2nd](https://www.oreilly.com/library/view/building-microservices-2nd/9781492034018/)
