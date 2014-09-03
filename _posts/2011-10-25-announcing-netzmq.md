---
title: Announcing netzmq
layout: post
redirect_from:
 - /post/14298996994/announcing-netzmq
---

Whether or not it was necessary, the world now has another set of .NET bindings for [ZeroMQ](http://www.zeromq.org/).

[netzmq][netzmq-repo], a project that I've been working on for the past two months, is ready for alpha testing. It has not been hardened for production use, but that's what I plan to do for the next two months (or so).

If you're feeling brave, you can reference the [NuGet package][netzmq-nuget] or clone the [repository][netzmq-repo].

## Doesn't this already exist?

Short answer: no with an "if", long answer: yes with a "but".[^rev]

The [official .NET bindings](https://github.com/zeromq/clrzmq2) work. They are still officially maintained, though the pull requests are slowly piling up. The API itself strays from modern, idiomatic C# in that it relies on global static variables and does not provide interfaces, which makes unit testing quite challenging. Finally, the Any CPU platform is not supported[^pull], so client projects must target x86 or x64 explicitly.

These are not deal breakers.

Maintainers can be added to deal with pull requests. Client projects can create socket wrappers for testing purposes. And x86 is [probably good enough][anycpu-trouble] until we start sending 2GB+ messages, at which point we can recompile for x64.

But, as is the wont of a na&iuml;ve programmer, I decided to toss it all and start from scratch. The result is netzmq.

## Why use netzmq?

Allow me to enumerate:

1. Clean API &mdash; behavioural interfaces, inversion of control and no global variables. Test and mock to your heart's content. Note that client projects must manage ZMQ Context lifetime scopes manually, which a good DI library can help with.

2. Any CPU &mdash; it can be advantageous for .NET projects to target the Any CPU platform, especially in heterogeneous deployment environments. Since ZMQ is a native C library and Windows does not support universal binaries, libzmq must be compiled separately for 32-bit and 64-bit platforms. Normally, this restriction would bubble up to netzmq, but I circumvented it with <del>magic</del> a common technique that is beyond the scope of this post. The downsides are increased complexity on the native/manged boundary and a small performance hit on application start, but the resulting flexibility is worth it.

3. Performance &mdash; instead of traditional P/Invoke interop, netzmq uses a C++/CLI proxy project to interface with libzmq. Aside from a 10-15% performance boost[^cit], this also keeps the API layer decoupled from the interop layer (read: more testable). In the future, a P/Invoke-based interop layer can be added to target the Mono CLR, which does not support C++/CLI Mixed mode.

More fact than feature, but netzmq currently targets libzmq-3.0, which is nearing an official release. An effort will be made to always support the latest stable libzmq while providing support for legacy releases, where appropriate.

## The road to 1.0

The ZMQ API is quite small, so netzmq already supports all core functionality and the basic devices. Most of the work in the next couple months will be writing documentation and examples, as well as a P/Invoke-based interop layer for Mono support. Along the way, the ZMQ lat/thr programs will be added for your benchmarking pleasure.

Now for a *caveat emptor*: until netzmq has been rigorously field tested, **please do not use it in mission-critical systems**. Play with it, break it and log issues on [GitHub][netzmq-repo], but do not use it to build your multi-million dollar high frequency trading system.

At least not yet.

[^rev]: Reverend Lovejoy, paraphrased
[^pull]: My <a href="https://github.com/zeromq/clrzmq2/pull/23">pull request</a> deals with this, albeit naively. netzmq's implementation is more robust.
[^cit]: \[Citation needed\]

[netzmq-repo]: https://github.com/jgoz/netzmq
[netzmq-nuget]: http://nuget.org/List/Packages/netzmq
[anycpu-trouble]: http://blogs.msdn.com/b/rmbyers/archive/2009/06/09/anycpu-exes-are-usually-more-trouble-then-they-re-worth.aspx
