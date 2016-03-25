---

layout: post
title:  "The beginner's guide to protocols and protocol extensions in Swift"
date:   2016-03-03 20:18:57
group: blog
summary: todo

---

There has, without doubt, been a big focus on protocols, protocol extensions and idea of protocol-oriented programming since the launch of Swift launched. Even though protocols (an interface) and the protocol extensions (a default implementation for the interface) can seem simple at first, it's actually a pretty powerful feature of Swift. For me, the idea of conditionally extending (using the `where` clause) is really cool. I've felt that I had a good idea of how it worked, but it wasn't until recently I really started playing with it. It was a tough but mind-blowing experience. I hope to get any newcomer to these concepts up to speed with this blog post, as I try to introduce some of the basic concepts I learned while experimenting.   This post will not go into depth about the concept, but more focus on practical examples to inspire any interested reader.

For more background knowledge on the subject, have a look at some of the following resources:

- [Protocol-Oriented Programming in Swift @ WWDC 2015](https://developer.apple.com/videos/play/wwdc2015/408/){:target="_blank"}
- [Ray Wenderlich's introduction to POP](https://www.raywenderlich.com/109156/introducing-protocol-oriented-programming-in-swift-2){:target="_blank"}
- [Mixins over Inheritance](http://alisoftware.github.io/swift/protocol/2015/11/08/mixins-over-inheritance/){:target="_blank"}
- [iOS 9 Tutorial Series: Protocol-Oriented Programming with UIKit](http://www.captechconsulting.com/blogs/ios-9-tutorial-series-protocol-oriented-programming-with-uikit){:target="_blank"}


# Composition over inheritance

todo