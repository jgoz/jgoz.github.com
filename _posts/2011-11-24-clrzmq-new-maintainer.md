---
title: New clrzmq Maintainer
layout: default
category: Code
tags: [clrzmq, netzmq, zeromq]
redirects:
 - /post/14299607562/new-clrzmq-maintainer
---

Less than a month after I [released][netzmq-release] [netzmq][netzmq-repo], the ZeroMQ team added a new maintainer to the [official .NET ZeroMQ bindings][clrzmq-repo]: me.

I'm not exactly sure how I got on the shortlist, but the keys have been handed over and the locks have been changed[^lie]. As you might imagine, this role conflicts with the work I'm doing on netzmq. To what extent, I can't really predict, but netzmq will be moved to the back burner for a while.

But it is not being abandoned.

<!-- end_preview -->

netzmq was always intended to be a production-worthy experiment and I will continue to use it on personal projects. It will still be actively developed and maintained, just at a slower pace. The best case scenario is that ideas from netzmq will make their way into future versions of clrzmq.

But enough about the new-old, let's talk about the old-new.

## Short-term clrzmq goals

Along with the keys to the shop, I was given a short term TODO list that looked something like this:

1. Pull requests
2. Issues
3. Documentation
4. Tests
5. General cleanup

As of this second, I'm somewhere between 2 and 3. My pull request and issue triage (or perhaps "rampage") is nearly complete. Basic documentation will follow, as will basic functional tests. General cleanup will happen throughout.

There is one feature, though, that I do plan to implement: Any CPU platform support. It will **not** match the netzmq approach; it will look more like my original pull request that was never merged. But instead of completely replacing the P/Invoke approach with a LoadLibrary approach, I will make them coexist as separate build and deployment options.

After this has been implemented, you can expect an Any CPU clrzmq NuGet package in addition to the x86 and x64 packages. And, of course, there will be ample documentation on setting up Any CPU, as there are a few gotchas.

At this point, full backward compatibility will be ensured as I make changes to clrzmq2. But after the short term goals have been met, I plan to start working on the next major version of the official bindings.

## Long-term clrzmq goals

These are not as well defined as the short term goals, but there is a list of features I'd like to add, in no particular order:

- ZeroMQ 3.0 compatibility
- Better Any CPU support (similar to netzmq, if possible)
- More testable API
- Optional C++/CLI interop layer for Windows
- Better Device API, included in the core library
- Adherence to the Microsoft API guidelines wherever possible[^naming]
- An official website

Some may be loftier than others, but nothing in the above list is impossible or heretical. There is no reason that ZeroMQ shouldn't have first-class support in .NET and Mono.

Once clrzmq2 is in tip-top shape and the ZeroMQ team has decided whether to branch or start fresh on the next version, I'll post another update.

So until then, take comfort in the fact that your pull requests and issues will go unnoticed no longer!

[^lie]: Not really.
[^naming]: This will affect class names. For example, `Context`, `Socket` and `Exception` are poor class names in 3rd party libraries because they exist in the .NET system libraries. `ZmqContext`, `ZmqSocket` and `ZmqSocketException` are better names.

[netzmq-release]: /2011/announcing-netzmq/
[netzmq-repo]: https://github.com/jgoz/netzmq
[clrzmq-repo]: https://github.com/zeromq/clrzmq2
