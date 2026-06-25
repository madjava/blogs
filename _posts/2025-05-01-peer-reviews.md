---
layout: post
author: Felix Eyetan
title: Peer reviews and why we need them
level: Beginner
is_blog: true
tags: [DevOps]
---

Code reviews are methodical assessments of code designed to identify bugs or errors, increase code quality, and help developers and engineers learn the source code.

PRs can also serve as a learning tool or a means of documentation. As not everyone in working on the same Initiative or deliverables at the same time, the opportunity to review another team members code give you the opportunity to be in the know of whats happening on the project.

## Effective Peer Review

Below are some ideas to explore in your professional journey when working with a group of engineers or collaborating with some one on a body of work. These ideas are based on my experiences on various projects, past and present, and i hope you would find some of these ideas helpful on your projects.

### 1. Keep PRs small, limit the code to one cohesive change

Short pull requests (or PRs) are much simpler to review than long PRs. Try to break up large features or changes into multiple smaller PRs, more bugs and issues slip through on longer PRs due to reviewer fatigue. Also bigger PR longer time to be reviewed. Best to get small changes out than one giant change.

### 2. Follow consistent code style

Following specific guidelines will allow your reviewers to focus on the substance of your change rather than superficial issues - haggling over whitespaces, funky indentations, style or format of function names is not a great use of anyone’s time.

### 2. Make your code self-documenting

Think about your variables, function or resource names, signatures, modules, etc. Make sure they accurately describe their contents and purpose. Using good naming convention can make your code substantially easier for your reviewers to understand. Whether it's IaC in Terraform or a feature in Java or NodeJS. Principles remain the same.

### 3. Document complex parts of the code with comments that show your intent as an engineer

In those cases when it’s not possible to write self-documenting code, it is sometimes complicated or hard to follow just by reading it, a few short comments describing how the code works and why it is the way it is can help, even if its eventually removed after reviews and before your merge. 

Caution though not to over load your code with comments, this is where `README` files in your project can come in handy, especially if you introduced significant change to the project or functionality.

### 4. Give your PR a good title and PR description

Be specific, describe both the reason for the change and the way this change achieves a goal. Be clear, but not overly verbose. A clear description helps your reviewers gain context on what you’re looking to achieve.

### 5. Draw attention to sections of interest

Provide pointers as comments for sections of the code that could use particular attention from the reviewer. Dropping comments on your own PRs can allow you to provide justification for changes in-line. This is helpful when you have thoughts that you need to share but that wouldn’t be useful as a persisting code comment, describing the reasoning behind an approach can sometimes make more sense as a comment than as a comment in the source code. Super useful as well if you'd like other opinions on a particular implementation.

### 5. Request reviews from the right people

Sometimes, asking for reviews from people with the right context, experience or knowledge of the project can be helpful. 

General rule of thumbs is that any member of the team should be able to review, but honestly i've been in situations where i had to pull in specific team members on an issue due to experience in that topic or feature. Value every review though.

### 6. Indicate when your code is ready for review

You can save your reviewers’ time and yourself from frustration by marking your PRs as a `Draft` while you are still working on it and only tagging people or requesting a review when your code is ready for a look.

This saves a team member having to "down tools" to review your work and also "comments" about things you know you are going to implement anyways but not gotten to it yet.

Always nice to start out in `Draft` mode then when ready for first review switch to `Ready for Review`.

### 7. Ask for the feedback you’re looking for

It can be helpful to write your reviewers a little note as a comment on the PR pointing out areas of uncertainty you have, requesting specific types of suggestions, or waving them off of something if you have broken a standard pattern for a valid reason.

Leverage comment on your PR at to draw attention to point for feedback.

## General Standards, Best practice

### Change Requests

Once you request a change i.e. `Block` a PR from merge, "You own it", reach out if no updates after a while or help the engineer with pointers especially if they are struggling. The time taken to update the PR, especially if not a very complex issue/resolution is requested, can be an indication that the engineer is struggling in implementing the requested change.

I have experienced a situation where a team member requested a change, essentially `blocking` my PR but then went on leave without getting back to review changes before heading out.

### Professional Courtesy

Don’t merge until reviewers' comments are clarified, where possible. If a colleague has made the effort to review and has asked some clarifying questions, just because you have the "required" number of approvals does not mean you totally ignore their question(s). 

Do your best, and as a matter of courtesy, to either respond to the comment or reach out via Slack or whatever internal messaging channel is in use to chat about it.

### Description-less PRs

Engineers are sometimes in a hurry or maybe feel the change is a very small change and they can't be bothered. This is not a good practice. At least add the link to the Jira ticket, if no ticket, there usually is one, then mention the incident or change request number or any other available tracking detail. If none of the above exist then state clearly the reason for the PR, there has to be one. 

There are no "useless" descriptions either, your future self and other engineers, current and future will appreciate the effort.

### Branch / PR prefix

Making it possible to find or reference changes when looking through your git history can be very helpful. Some ideas when creating your branch include:

- `<jira number>-<branch name>`
- `<issue number>-<branch name>`
- `<incident number>-<branch name>`
- `<change request number>-<branch name>`

A colleague of mine uses `fix:` to indicate a fix or `feat:` to indicate a feature to help him differentiate what type of change when in.

If your organisation is not fussy about this or there isn't a standard your team follows then i propose one of the above ideas.

It's always best to avoid "floating" PR that make no sense 3 months later.

### Leave a comment on the Jira ticket before you mark is as "Done"

If you are in a position where you can or are allowed to close Jira tickets, it's good to be proactive, add a small comment when closing a ticket item. 

Your Agile lead or Project manager will thank you for it. Useful details/information in your Jira ticket or GitHub project Issues before closing them can go a long in providing context to the wider team, external or interested parties who usually are nontechie's folks.

### Complete your reviews

Because some other team member has approved a PR does not mean you should not complete your review especially if it’s a familiar code base or project.

I have noticed on projects i've worked on where some other engineer abandons his/her review just because more "senior" or "experienced" engineers have give it a thumbs up.

From experience they sometimes are wrong and sometimes miss details, usually because they usually are always "multitasking", so many other "issues" needing their attention because of their experience.

If not for anything, try understand why that "senior" or "more experienced" engineer thought the PR is good-to-go. You can gain experience for next time on similar PRs, also you may just notice they have missed a point and your attention to detail prevented a bug making it's way to production, especially if working on a full CI/CD project.

### Patience, waiting for Reviews

Sometimes waiting for a PR review can be nerve racking. If on a busy project or availability is limited due to holidays or absence, you'll need to be patience and considerate when "pinging" others to to get some "eye's on it".

Some ideas i have used in the past, while i wait, is to "where" a different "hat" and look at my PR again as a "reviewer" or i ask myself "what can i improve on?". I sometime find more things to do to improve what i thought was "ready for review".

### Have a "thick" skin

After all, you asked for it (joke), keep an open mind. Some colleagues don't have filters and come at you quite bluntly, no "sugar coating". Keep the end goal in mind, at least consider their point, sometimes they are actually right and sometimes not, always best to consider other ideas or thoughts, your end result will be much better and by thinking through it with other ideas in mind you may just learn something new as well.
