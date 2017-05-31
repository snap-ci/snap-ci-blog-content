---
layout: post
title:  "Are Your Team Practices Accidentally Making Things Harder for Introverts?"
description: "Just because an activity is draining doesn’t mean you shouldn’t do it, you should just make sure it is the most effective use of your team’s energy."
date:   2016-05-10
author: Jeff Norris
index-image-url: screenshots/continuous-integration-introverts/continuous-integration-introverts-happy-team.jpg
index-image-alt: 'continuous integration and introverts'
keywords: snap-ci, continuous delivery, software development, devops, trunk based development,
categories: TEAMS CONTINUOUS-INTEGRATION
---


## Can continuous integration improve life for introverts?

A common definition of an introvert is someone who is energized by spending time alone. This does not mean that they don’t work well in groups or that they need to be locked in a quiet room by themselves (like I am right now writing this) to to be productive. It just means that some team activities may be fun and energizing to certain members, but draining to others. Just because an activity is draining doesn’t mean you shouldn’t do it; you should just make sure it's the most effective use of your team’s energy.

Let’s say you have two members of your team, Indy the introvert and Elsa the Extrovert.  

![Indy discovers his own artifacts](/assets/images/screenshots/continuous-integration-introverts/continuous-integration-introverts-artifacts.jpg){: .screenshot .big}

Both Indy and Elsa have each spent the last two weeks working on really important features for your product to manage Archeological Expeditions Inc. They have both been regularly committing their code to their feature branches, and are ready to merge back into the master branch. Unfortunately, there are some major merge conflicts in regards to how artifacts from the expeditions are stored. To resolve the issue, you have a meeting with the whole team to hash out all the details. It is a long, drawn out meeting. Elsa dominates the conversation because she really enjoys having lively debates. As a result, she ends up getting her way, since Indy decides after answering the third round of objections that it is too much work to fight it out. Elsa seems to think of the debate as a fun verbal joust, while Indy thinks of it as a bitter battle.

![Elsa has her own artifact](/assets/images/screenshots/continuous-integration-introverts/continuous-integration-introverts-discovery.jpg){: .screenshot .big}

How could this situation have been handled differently? Before we start, we acknowledge that both Elsa and Indy care a lot about Archeological Expeditions Inc. They want the product to be successful and are doing everything they can to make sure that it is. They just don’t always see eye-to-eye on how to achieve that goal and conflict comes out of trying to resolve the disconnect.

### Identify the conflict early

When the conflict is identified early, it's relatively easy to fix. If it's not caught early, it gets exponentially worse, building more functionality on top of their differences. So to avoid conflicts, they actually need to encourage having really small conflicts a lot earlier. They need to bring their changes back to a shared master/trunk on a regular basis, at least once a day. They could either move towards trunk-based development or--if they really want to keep working in their branches--they need to merge their code back into trunk regularly. The usual way that people would do this is to merge their code with the trunk and run all the tests on the merged code. If it passes all the tests, then they can confidently merge their code back into the trunk. In the Snap CI tool, this is done automatically using branch tracking. This small change lets Elsa make her changes to how the artifacts are stored, then Indy can see that there is a disconnect right away, rather than waiting until both of them have made substantial changes that rely on their different assumptions.

### Be specific and be sensitive

Conflicts are much less painful when they are specific and one-on-one instead of general and in a group setting. Large team discussions can swerve from a facts-based discussion of the issue at hand to a dramatic conflict with charged emotions. Elsa and Indy both want to have the best possible solution to storing artifacts. If they get into a giant argument, there is a good chance that Indy will feel personally attacked or that Elsa may think that she is being accused of being a bad teammate because she didn’t take Indy’s needs into account. If they had broken the disagreement up into several small discussions, then they would have been able to resolve them one at a time and would have come to a shared understanding earlier. Once they agree where artifacts should be stored, then they can go back to focus on their own parts of the system.

### Break changes up into small, manageable pieces

By breaking their changes into smaller pieces and deploying them separately, Indy and Elsa can get value from their changes faster. Because of his quiet personality, Indy might not make a big deal out of the valuable things that he is doing to the system, while Elsa finds it very easy to talk about her successes. Having smaller, more regular builds allows the team to see the value of Indy’s contributions in working software faster. It also lets the team find bugs faster and isolate exactly when they happened. Because they are getting feedback faster, they can incorporate that feedback to quickly change direction.

![Continuous Integration helps them work together](/assets/images/screenshots/continuous-integration-introverts/continuous-integration-introverts-happy-team.jpg){: .screenshot .big}

## When consistently applied, CI practices moves team communication forward

By paying attention to the personalities of your teammates, you can make substantial improvements to their well-being as well as to overall productivity. You want to make sure that they are spending their energy on things that really matter. You also want to make sure that decisions are made based on what is best for the team overall, not just who is the best debater. When consistently applied, continuous integration practices help the team bring communication forward so team members can have simple, straightforward discussions and come to a shared understanding sooner.   

 
Snap CI © 2017, ThoughtWorks
