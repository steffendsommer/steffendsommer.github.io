---
layout: post
title:  "Swift 3 and the lack of recursive protocol constraints"
date: 2016-10-08
group: blog
summary: While starting to understand the power of Swift, protocols and associated types I also started running into the limitations of the current version of Swift (version 3). One of the more surprisingly limitations I've faced is the one related to recursive protocol constraints.
---

While starting to understand the power of Swift, protocols and associated types I also started running into the limitations of the current version of Swift (version 3). One of the more surprisingly limitations I've faced was when I got the `Type may not reference itself as a requirement` error. It's actually a bit annoying as this is a very common thing to do in a project, but for some reason, this hasn't been added to the awesome Swift language yet.

Alright, so it all boils down to recursive protocol constraints. Meh? One protocol referencing another protocol and vice versa. It could be when you try to explain a relationship between your `View` and your `ViewModel` if you're running a MVVM architecture, but I guess this applies to a lot of different relationsships. For this example, I will continue with the idea of a `View` referrencing a `ViewModel` and a `ViewModel` referrencing a `View`:

```swift
protocol ViewModelType {
    associatedtype V: ViewType
    var view: V { get }
}

struct ViewModel<V: ViewType>: ViewModelType {
    var view: V
}


protocol ViewType {
    associatedtype VM: ViewModelType
    var viewModel: VM { get }
}

struct View<VM: ViewModelType>: ViewType {
    var viewModel: VM
}
```

This is a pretty straightforward, and to increase the flexibility in our code, we depend on the `..Type` variants. However, the above code will give you the mentioned error: `Type may not reference itself as a requirement`.

After digging into this issue and reading about people's ideas for workarounds, I managed to silence the compiler by doing a small hackish workaround:

```swift
protocol _ViewModelType {}
protocol ViewModelType: _ViewModelType {
    associatedtype V: _ViewType
    var view: V { get }
}

struct ViewModel<V: ViewType>: ViewModelType {
    var view: V
}


protocol _ViewType {}
protocol ViewType: _ViewType {
    associatedtype VM: _ViewModelType
    var viewModel: VM { get }
}

struct View<VM: ViewModelType>: ViewType {
    var viewModel: VM
}
```

Good for now, but as soon as you want to use these types and you need to specialize them you end up in a never-ending-specialization-loop:

```swift
let vm = ViewModel<View<ViewModel<View...>>>()
```

The only thing we achieved was to postpone the problem. At this point I was like: Compiler..

 ![say-to-my-face](/assets/posts/say-to-my-face.gif)

After some more digging, I finally got it confirmed that this is a current limitation in Swift. Check out the open issue [here](https://bugs.swift.org/browse/SR-1445) and the discussions about this topic [here](http://stackoverflow.com/questions/37253236/swift-protocol-with-associated-type-type-may-not-reference-itself-as-a-require), [here](https://forums.developer.apple.com/thread/15256) and [here](http://stackoverflow.com/questions/31869440/swift-xcode-7-beta-5-type-cannot-refer-to-itself-as-a-requirement).

[I reached out on Stack Overflow](http://stackoverflow.com/questions/39012959/working-around-the-lack-of-recursive-protocol-constraints-in-swift-3) for suggestions on a temporary workaround until Swift supports this. Unfortunately there has not been added any answers, but the number of up-votes and stars made me think that others is struggling with this as well.

Anyways, I needed a temporary workaround and the best I could come up with is the following, which also can be seen on the SO post:

```swift
protocol ViewModelType {
    associatedtype D: Any // Compromise to avoid the circular protocol constraints.
    var delegate: D? { get set }
}

protocol ViewType {
    associatedtype VM: ViewModelType
    var viewModel: VM { get }
}

protocol ViewDelegate {
    func foo()
}


struct ViewModel: ViewModelType {
    typealias D = ViewDelegate
    var delegate: D?

    func bar() {
        delegate?.foo() // Access to delegate methods
    }
}

struct View<VM: ViewModelType>: ViewType, ViewDelegate {
    var viewModel: VM

    func foo() {
        // Preferred, but not possible: viewModel.delegate = self
    }
}


var vm = ViewModel() // Type: ViewModel
let v = View(viewModel: vm) // Type: View<ViewModel>
vm.delegate = v
```

The main idea is to break the recursiveness by making someone conforming to a type like `Any`. This is not ideal, but an acceptable compromise for now. That someone is in my example a `delegate` which basically is some kind of "mediator object" that handles the referrences between the `View` and the `ViewModel`. The only downside of having this `delegate` is that it needs to be set up from the "outside", from where the objects are created, and cannot be set in the View implementation. If one tries to do this, the error `Cannot assign value of type View<VM> to type _?` will appear.

However, with this approach we get the correct types without having to do a lot of specialization. Of course, one could add more protocols to have even more abstractions, but the same solution would apply.