---
layout: post
author: Felix Eyetan
title: Peer reviews and why we need them
level: Beginner
is_blog: true
---

# Introduction

Code reviews are methodical assessments of code designed to identify bugs, increase code quality, and help developers learn the source code.

## Effective Peer Review

Below are some ideas to explore in your professional journey when working with a group of engineers. These ideas are based on my experiences on various projects, past and present and i hope some of them will be helpful to you on your project.

### Keep PRs small, limit the code to one cohesive change

Short pull requests (or PRs) are much simpler to review than long PRs. Try to break up large features or changes into multiple smaller PRs, more bugs and issues slip through on longer PRs due to reviewer fatigue.

### Follow consistent code style

Following specific guidelines will allow your reviewers to focus on the substance of your change rather than superficial issues - haggling over whitespace is not a great use of anyone’s time.

### Make your code self-documenting

Think about your variables function names and signatures and make sure they accurately describe their contents. Using good naming with short methods can make your code substantially easier for your reviewers to understand.

### Document complex parts of the code with comments that show your intent as an engineer

In those cases when it’s not possible to write self-documenting code, it is sometimes complicated or hard to follow just by reading it, a few short comments describing how the code works and why it is the way it is can.

### Write a good PR title and PR description

Be specific, describe both the reason for the change and the way this change achieves a goal. Be clear, but not overly verbose. A clear description helps your reviewers gain context on what you’re looking to achieve.

### Provide pointers as comments for sections of the code that could use particular attention from the reviewer

Dropping comments on your own PRs can allow you to provide justification for changes in-line. This is helpful when you have thoughts that you need to share but that wouldn’t be useful as a persisting code comment, describing the reasoning behind an approach can sometimes make more sense as a comment on a diff than as a comment in the source code.

### Request reviews from the right people

Sometimes, asking for reviews from people with the right context on your change and experience with the tech stack can really make reviews go smoothly.

### Indicate when your code is ready for review

You can save your reviewers’ time and from frustration by marking your PRs as a draft while you are still working on them and only tagging people or requesting a review when your code is ready for a look.

### Ask for the feedback you’re looking for

It can be helpful to write your reviewers a little note as a comment on the PR pointing out areas of uncertainty you have, requesting specific types of suggestions, or waving them off of something if you have broken a standard pattern for a valid reason.

## General Standards, Best practice

### Change Requests

Once you request a change i.e. Block a PR from merge, "You own it", reach out if no updates after a while or help the engineer with pointers   especially if they are struggling. time taken to update can be an indication, especially if not a very complex issue/resolution is requested.

### Professional Courtesy

Don’t merge until reviewers' comments are clarified, where possible. If a colleague has made the effort to review and has requested some clarifying questions, just because you have the "required" number of approvals does not mean you totally ignore their question(s). Do you best to either respond to the comment or reach out via Slack or whatever internal messaging tool is in use to chat about it.

### Description-less PRs

Engineers are sometimes in a hurry or maybe feel the change is a very small change and they cant be bothered. This is not a good practice. At least add the link to the Jira ticket if no ticket (there usually is one) then mention the incident/change request number or any other available tracking detail. There are no "useless" descriptions either, your future self and other engineers will appreciate the effort.

### Branch / PR prefix

Making it possible to find or reference changes when looking through your git history can be very helpful. Some ideas when creating your branch include:

- `<jira number>-<branch name>` or
- `<incident number>-<branch name>` or
- `<change request number>-<branch name>`

A colleague of mine uses `fix:` to indicate a fix or `feat:` to indicate a feature to help him differentiate why type of change when it.

If your organisation is not fussy about this or there isn't a standard your team is meant to follow then i propose the above ideas.

It's always best to avoid floating PR that make no sense 3 months later.

### Leave a comment on the Jira ticket before you mark is as "Done"

If you are in a position where you can or are allowed to close Jira tickets, it's good proactive to add a small comment when doing so. Your Agile lead or Project manager will thank you for it. Useful details/information in your Jira ticket or GitHub project Issues before closing them can go a long if providing context to the wider team and external or interested parties.

### Complete you reviews

Because some other team member has approved a PR does not mean you should not complete your review especially if it’s a familiar code base.

I have noticed on other project where some other engineer abandons his/her review just because other more "senior" or "experience" engineers have give it a thumbs up.

From experience they sometimes are wrong and sometimes miss details, usually because they usually are always "multitasking" on any project, so many other issues needed their attention because of their experience, that fine, it comes with the job, thats why you are there. If not anything try understand why that "senior"/"more experience" engineer though the PR is good-to-go. Sometimes you will notice they have missed a point and your attention to detail save a bug making its way to production. Especially if working on a full CI/CD project.

### Patience, waiting for Reviews

Sometimes waiting for a PR review can be nerve racking. If on a busy project or availability is limited due to holidays or absence, you'll need to be patience and considerate when "pinging" others to have a look.

Some ideas i have used in the past it to "where" a different "hat" and look at my PR again as a "reviewer" or i ask myself "what can i improve on?". I sometime find more things to do to improve what i thought was "ready for review".

### Have a "thick" skin

After all, you asked for it (joke), keep an open mind. Some colleagues don't have filters and come at you quite bluntly, no "sugar coating". Keep the end goal in mind, at least consider their point, sometimes they are actually right and sometimes not, always best to consider other ideas or thoughts, your end result will be much better and by thinking through it with other ideas in mind you may just learn something new as well.
