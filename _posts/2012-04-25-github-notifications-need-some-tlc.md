---
title: GitHub Notifications Need Some TLC
layout: default
category: Code
tags: [git, rant]
redirects:
 - /post/21832824801/github-notifications-need-some-tlc
---

I love GitHub. A lot. I love it almost as much as I love complaining about things (foreshadowing!). It is the best thing to happen to the development workflow since distributed version control burst forth, fully formed, from Linus Torvalds's skull atop Mount Olympus[^dvcs].

But like most things we love, GitHub is not without its imperfections. Until recently, I only used it for personal and open source projects, so its email notification system worked logically. Notifications were sent to my personal email address, as I would expect, and I could use the GH-added mail headers to easily filter and organize messages by project.

Now that my company uses GitHub, however, the notification system has become a weak link. The problem is that all notifications for my company-related repos are still sent to my personal email address. As far as I know, there is no way to change that.

Since I'm a good little worker bee and I try not to hang out on Gmail all day, I had to set up forwarding filters for all work-related notifications so that they (correctly) get sent to my work email. This is not ideal.

<!-- end_preview -->

**Suggestion #1: Allow per-organization email addresses for notifications.**

This would let me delete my filters and keep my personal email free of work-related discussion. And before you say it, no, creating a separate profile for work is not a good solution. That's the whole point of Organizations.

The next issue is a result of our internal process, which requires a reviewed pull request before anything can be merged to master. This applies to all projects in the organization, so this generates a lot of notification chatter, as you might imagine. Even though I can review code from any project, I don't necessarily need to be notified on changes for all projects.

**Suggestion #2: Allow per-project notification toggles.**

This could be as simple as enabling/disabling notifications completely; it doesn't have to be as fine-grained as the main notification options. With this feature, I could enable notifications for the projects I'm actively working on and disable them for the other projects in the organization. I'd still be able to see all activity on GitHub itself, but my email wouldn't be cluttered with discussion that I may or may not be participating in. If I did jump on a pull request for another project, GH could automatically opt me into that particular thread as it currently does (I think).

With these two suggestions implemented, GitHub would become yet another step closer to the one-platform-to-rule-them-all. And it would probably shut me up for a couple of months, too. Everyone wins.

[^dvcs]: That's probably not an accurate history of DVCS.

