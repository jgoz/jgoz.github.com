---
title: Failing with git filter-branch
layout: default
category: Code
tags: [git, fail]
redirects:
 - /post/14489251688/failing-with-git-filter-branch
---

As tempting as it might be to, say, travel back in time to kill Hitler, the unforeseen consequences could be disastrous. The possibility of a [Flanders-ruled dystopia][simpsons] or [becoming one's own grandpa][futurama] should be enough of a deterrent for most would-be time travellers.

But last week, flipping caution an undeserved bird, I used `git filter-branch` in an attempt to not only kill Hitler, but _erase him from history completely_. Thus ends the Hitler metaphor and begins a tale of woe.

<!-- end_preview -->

After reading [Cleaning Up Your Git Repository for NuGet 1.6][article], my OCD was tingling at the thought of erasing some never-used binaries from one of our GitHub repos that already uses NuGet. Completely out-of-character, I blindly copy-paste-executed commands from the article to my terminal, did a `git push origin master --force` and saw that it succeeded with no errors. Great! I re-cloned the repo, just to make sure, and everything seemed fine and dandy.

That is when the universe ripped in half.

You see, I did not read the source material referenced in the article, which is a GitHub help page on [removing sensitive data from a repo][gh-help]. Here, it lists two vital pieces of information missing from the NuGet article:

1. `git filter-branch` only works on one branch at a time, so you may need to perform the cleanup on other branches as well. (Looking back, this should have been obvious.)
2. After collaborators fetch the new branch, they will need to use `git rebase` on their own branches to rebase them on top of the new one.

Not realizing #1, I only cleaned up our `master` branch, which (effectively) created a separate commit history for an unmerged topic branch in the repo. When it came time to merge this branch back to `master`, it would simply fail, even after resolving all the conflicts.

At this point, we were freaking out (and I was extremely embarrassed). This is where we should have collectively facepalmed and said "[rebase][rebase]!", following the guidance of #2.

Instead, we decided to `git cherry-pick` the commits from the topic branch onto `master`, which had the effect of a `git merge --squash`. It got the result we wanted, albeit without preserving the branch's commit history, so we deleted the unmerged branches and cut our losses.

Naturally, beer is on me this week.

After wallowing in my embarrassment, I decided to channel it into researching what I should have done. This has yielded the following set of rules for `git filter-branch`:

1. Don't use `git filter-branch`.
2. _Please_ don't use `git filter-branch`.
3. If you insist, make sure your topic branches have been merged back to master.
4. If that's not possible, prepare to [rebase][rebase] your topic branches.
5. Reconsider the whole thing again.
6. Don't say I didn't warn you.

But the ultimate lesson to be had is this: When rewriting history, one has to prepare for an infinite number of parallel universes in which you blow of your own foot with a shotgun. In this universe, the shotgun's name was "git" &mdash; as in, "don't mess with history, you stupid git."

[simpsons]: http://en.wikipedia.org/wiki/Treehouse_of_Horror_V#Time_and_Punishment
[futurama]: http://en.wikipedia.org/wiki/Roswell_That_Ends_Well
[article]: http://coderjournal.com/2011/12/cleaning-up-your-git-repository-for-nuget-1-6/
[gh-help]: http://help.github.com/remove-sensitive-data/
[rebase]: http://book.git-scm.com/4_rebasing.html

