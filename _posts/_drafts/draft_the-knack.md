---
title: The Knack is real...
date: 2024-12-05 11:22:33 +1000
categories: [Quality Engineering]
tags: [quality-engineering]     # TAG names should always be lowercase
---


![post avatar](/assets/avatar_16.png){: width="200" height="100" .right}

Ever had that friend, or relative who just seemed to... get it? 

You may have met someone with _**The Knack**_

The Knack is the ability to look at a system or machine and intuitively grasp how that machine works, or could be made to work again. As someone who has trained multiple (!) apprentices over my time in the mechanical trades this is a very real phenomenon.

**Symptoms may include:**
- They jerry-rigged a server in their spare laundry cupboard that serves equal part Tor node, part Plex server?
- They fixed their own lawn mower, which incidentally they found on the side of the road, or picked up off someone who just "couldn't get the damn thing to run".
- They have a raspberry pi. You have no idea why and often they don't either, but they have one and they needed it as soon as they saw it.
- Previous/current Lego nerd is a bonus point!

The group of people with The Knack are seemingly jack-of-all-trades. Give them a tool and a way to access it, and suddenly your system is in their hands and they're ready to let you know that such-and-such is loose, or this bit is deprecated and could do with an update.
These are the kind of people that make or break an organisation - one of the recent trends in software development has definitely been the adoption of coding by the wider mainstream. There's coding camps for kids, boot camps for career-changers and postgrads (even non-grads in some cases) and with the rise of "nerd culture" and the acceptance of living life online there's a real shift in the IT industry. Gone are the days of the life-long software devotee - almost every facet of our modern world brushes shoulders with software, and the necessary understanding of how that software works is increasingly bleeding into job descriptions everywhere.



> Tell me and I forget, teach me and I may remember, involve me and I learn - usually attributed to Ben Franklin but that's just US imperialism

```
Not having heard something is not as good as having heard it;
    having heard it is not as good as having seen it; 
    having seen it is not as good as knowing it 
    knowing it is not as good as putting it into practice - Xunzi by Xun Kuang
```

80/20 rule - what is it?

Positive language - assert true always for everything.

Truthy, what is the difference

assert cna show the caller fuunction? check this

Trusting tests and confidence - why confidence is better than correct.
Russian provedrd - "trust but verify"

Framework - make a test class containing all the test methods, then a single point to run the entire class worth. This means they can share their own data, like a context, and the whole class becomes the test, but you can break it down per method if neeeded. Run method can contain all the before//after steps etc. once the class is mad,e move the actal tests to subclasses and just keep the run methods etc. in the parent class. this can mean the actual problems are happenign in the parent class as the runner, but we can push it down to the subclass and morve the report to a reporter class - which we just pass the entire test object to for failed runs so we can retrieve the entire test result rather than jsut the pass/fail.
building a framewrok - start with the atom: assert something, build to molecules: assert multiple things, add methods to create a Test, add many tests to create a class, teach the class to run one, teach the class how to run many, teach it to run all test classes, give it reporting, error handling, then 
flay/flog what is this?





