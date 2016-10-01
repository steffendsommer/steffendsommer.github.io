---
layout: post
title:  "Objective-C’s @selector() in Swift"
date:   2016-03-24 09:53:28
group: blog
summary: "Swift 2.2 got released recently, and with the new version, Apple continues to show that they're committed to this language. One of my favourite new features of Swift is the #selector() method, which can make our code more safe."
---

Swift 2.2 got released recently, and with the new version, Apple continues to show that they're committed to this language. One of my favourite new features of Swift is the `#selector()` method, which can make our code more safe.


# A quick Objective-C recap

In Objective-C we have been used to be able to write:

```objc
SEL mySelector = @selector(viewDidLoad);
```

Which is awesome. Why is that? By having the `@selector()` method, we're able to make the compiler have our back when referencing selectors. It might not be super clear, when we're referencing methods from the Cocoa API, but what if we referenced our own methods:

```objc
- (BOOL)validateString:(NSString *)myString {
    // Some validation code..
    
    return true;
}

// Some other place in our code:
SEL mySelector = @selector(validateString:);
```

In the above case, if the method named `validateString:` changed its name to e.g. `validateUsername:`, then the compiler would give us a warning. This is super useful, in contrast to if we created our selector from a string:

```objc
SEL mySelector = NSSelectorFromString(@"validateString:");
```

If we created our selector from a simple string and the method name changed, then the compiler will not warn us, and this will lead to crashes due to wrong selector references.


# #selector() in Swift 2.2.

Up until now, it has not been possible to do the same in Swift as `@selector()` did for us in Objective-C. Earlier, we've been forced to reference selectors doing:

```swift
let mySelector = Selector("viewDidLoad")
```

But since Swift 2.2 we are now (finally) able to get the help from compiler by using the newly introduced `#selector()`:

```swift
let mySelector = #selector(viewDidLoad)
```

Which, in my opinion, is pretty awesome 👌

Check out the rest of the new stuff in Swift 2.2 [here](https://swift.org/blog/swift-2-2-released/) and also check out [this blog post](https://medium.com/swift-programming/swift-selector-syntax-sugar-81c8a8b10df3#.z2cpx1xsa) for more syntactic sugar when working with this new feature.