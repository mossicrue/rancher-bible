# Don't Panic!

Things go wrong!

Failure, when it's binary, is often easier to diagnose than failures that are intermittent or that happen as a side effect of something else that hasn't completely failed.

When this happens in Kubernetes, or in any microservices application, troubleshooting can be difficult if you don't know where to look.
Troubleshooting a failed system can be frustrating, but there are some things to remember:
- Every problem has a solution.  
You may not have found it yet, but it’s there. If you remember this, it will help reduce your frustration and leave you with more energy to spend on solving the problem.

- A second set of eyes will often see things that you cannot.  
If you’ve been looking at a problem for more than ten or fifteen
minutes, ask someone else to take a look at it with you. You’ll be
amazed how often they’ll see the solution right away.

- If something worked before, and now it doesn’t, ask yourself: “What changed?”  
Consider everything, no matter how small. Review those changes and see if rolling them back resolves the issue.

- Change one thing at a time and then test.  
If you change multiple things and the problem goes away, you won’t know which thing was the cause or the solution. When you only change one thing at a time it will help you identify the factors that created the problem in the first place.

- No problem goes away on its own.  
If the problem vanishes as mysteriously as it arrived, know that it can return just as mysteriously. Continue trying to find a cause for it, applying the “what changed” question to the latest state of the cluster - what might have changed to make the problem go away? Was that also something that changed when the problem appeared?

- If you’re no longer able to view the problem objectively, which
means that if you’re frustrated and angry and emotional, take a
break. You will be more effective at solving the problem after you
return than you would have been in the time that you’re away.
Get up from your chair and move around the room. Take several
deep breaths. Relax. Remember the first item in this list - the
solution is there. Oftentimes a change in your environment as
simple as walking outside or going for a walk around the block is
enough to trigger new ideas on how to solve the issue.
