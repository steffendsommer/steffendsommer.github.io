---
layout: post
title: "TodaysReactiveMenu - An example app using ReactiveCocoa 3.0, MVVM and Swift"
date: 2015-06-02 18:06:28
group: blog
related-talks:
- /talks/getting-up-to-speed-with-reactivecocoa-version-4
summary: I've been working with ReactiveCocoa the last 8 months and I'm overall very satisfied with the framework and the approach in general. I've been following the progress on ReactiveCocoa 3.0 (mainly focused around Swift) and when it recently hit beta-4, I decided to dig in.
---

TL;DR The source code can be found here: [https://github.com/steffendsommer/TodaysReactiveMenu ](https://github.com/steffendsommer/TodaysReactiveMenu)

I've been working with ReactiveCocoa the last 8 months and I'm overall very satisfied with the framework and the approach in general. I've been following the progress on [ReactiveCocoa 3.0](https://github.com/ReactiveCocoa/ReactiveCocoa/tree/swift-development) (mainly focused around Swift) and when it recently hit beta-4, I decided to dig in.

First of all, I want to put out a disclaimer. I'm new to Swift and I'm definitely new to RAC 3.0. Because of that, the majority of the hours I put into making this app, I felt like I had no idea of what I was doing:

![I have no idea what I'm doing](/assets/posts/1432669225790.gif)

Also, it's worth noting that the intention of this project and blog post is just to get any interested reader kickstarted with ReactiveCocoa 3.0. It is not an introduction to ReactiveCocoa or MVVM. It might be easier to understand this post, if you've read the [RAC 3 changelog](https://github.com/ReactiveCocoa/ReactiveCocoa/blob/swift-development/CHANGELOG.md) beforehand.

# A small API for this purpose
Using rails and some mad scraping skills I created a small backend to parse  and expose the list of menus. The API can be found [here](http://unwire-menu.herokuapp.com/menus), and using a `limit` parameter [here](http://unwire-menu.herokuapp.com/menus?limit=1).

The limited version will result in a JSON response similar to this:

```
[
    {
        "cake": false,
        "created_at": "2015-05-29T06:58:09.000Z",
        "id": 241,
        "identifier": "1070757152939264",
        "link": "http://www.facebook.com/Cuisine.dk/posts/1070757152939264",
        "main_course": "Helstegt okseculotte og kartoffelsalat med chili og purløg.",
        "serving_date": 1432857600,
        "sides": "Chilimix. Brune ris med pærer, koriander, rejer og chili. Blandet salat med dressing. Dagens råkost med ananas. Knoldselleri med citron, æble, æbleeddike og persille. Spinatsalat med frisk mango og chili. Pålægsfad med afskåret pålæg, hjemmelavede salater samt. 3 slags ost. dagens kage. Friskbagt solsikkerugbrød, maltbrød og græskarkernebrød. Frisk frugt.",
        "updated_at": "2015-05-29T07:30:09.035Z"
    }
]
```


# The model
The purpose of the app is to show whatever is on the today's menu of my workplace. Interesting attributes could therefore be:

- An identifier
- A link to the menu (if any exernal resource exist)
- A date for serving
- The main course
- The sides
- And if there's going to be served cake

Using [ObjectMapper](https://github.com/Hearst-DD/ObjectMapper), and the built-in `DateTransform()`, this can be fairly simple mapped:

```swift
var identifier: String?
var link:       String?
var servedAt:   NSDate?
var mainCourse: String?
var sides:      String?
var cake:       Bool?

func mapping(map: Map) {
    identifier  <- map["identifier"]
    link        <- map["link"]
    servedAt    <- (map["serving_date"], DateTransform())
    mainCourse  <- map["main_course"]
    sides       <- map["sides"]
    cake        <- map["cake"]
}
```

# Talking to the API
Noticing [built-in extensions](https://github.com/ReactiveCocoa/ReactiveCocoa/blob/swift-development/ReactiveCocoa/Swift/FoundationExtensions.swift#L29-L50) for `NSURLSession` in ReactiveCocoa 3, I decided not to include any external networking frameworks. Using these extensions and the [Result framwork](https://github.com/antitypical/Result), fetching the today's menu seems like a breeze:

```swift
func fetchTodaysMenu() -> SignalProducer<Menu?, NSError> {
  
  let session = NSURLSession.sharedSession()
  let request = NSURLRequest(URL: NSURL(string: "http://unwire-menu.herokuapp.com/menus?limit=1")!)
  
  return session.rac_dataWithRequest(request)
    |> map { data, response in
      let json = try { (NSJSONSerialization.JSONObjectWithData(data, options: nil, error: $0) as? [NSDictionary])?.first }
      return Mapper<Menu>().map(json.value)
  }
}
```

# Preparing data for the view
Now for the fun part! So, the view model is responsible for any massaging of data while exposing whatever the view needs to display. This is often here you would do a lot of chained operations for making your code reactive and to achieve the content you want. Being new to both RAC 3 and Swift, this is here I struggled the most.

## The basics
Let's start with covering some of the most basic questions:

- Use `self.something <~ self.anotherThing` instead of `RAC(self, something) = RACObserve(self, anotherThing)`.
- Use `someResult |> filter { someVar in return someVar }` instead of `[someResult filter:^BOOL(id someVar) { return someVar; }];`
- Look into `ConstantProperty`, `MutableProperty` and `SignalProducer` for making these RAC bindings.
- Yes, you need to be a lot more strict with handling your types. (Dont worry, you will start to like it).

### Declaring a RAC property
Having these fundamentals in place, let's look into some binding examples. The first one, being a constant that never changes:
`let headline = ConstantProperty<String>("Today's Menu")`

### Content manipulation
The next one, being a minor manipulation using `map`:

```swift
private let menu = MutableProperty<Menu?>(nil)
let sides = MutableProperty<String>("")

self.sides <~ self.menu.producer
    |> ignoreNil
    |> map { menu in
        menu.sides!
    }
```

Note how the `ignoreNil` is used as `menu` might be `nil`.

### Signal manipulation
The last example looks into solving the following challenge: to fetch today's menu every time the view gets active.

Let's start out by declaring two helpers, as we're not using [ReactiveViewModel](https://github.com/ReactiveCocoa/ReactiveViewModel):

```swift
let active = NSNotificationCenter.defaultCenter().rac_addObserverForName(UIApplicationDidBecomeActiveNotification, object: nil).toSignalProducer()
    |> ignoreError
    |> map { _ in
        true
    }
let inactive = NSNotificationCenter.defaultCenter().rac_addObserverForName(UIApplicationWillResignActiveNotification, object: nil).toSignalProducer()
    |> ignoreError
    |> map { _ in
        false
    }

self.viewIsActive <~ merge([active, inactive])
```

Two signal producers to tell us whether or not the view is active. Note how `toSignalProducer()` has been used to go from `RACSignal` to `SignalProducer`. Note how `ignoreError` is being used to tell the compiler that we only care about values, not errors. The `ignoreError` and `merge` operator are small helpers I made during this project and is located [in the repo](https://github.com/steffendsommer/TodaysReactiveMenu/blob/master/TodaysReactiveMenu/Util/RACHelpers.swift) as well.

Having the knowledge for when the view is active, let's try to use this for triggering a menu fetch:

```swift
self.menu <~ self.viewIsActive.producer
    |> filter { isActive in
        return isActive
    }
    |> flatMap(FlattenStrategy.Latest) { _ in
        return fetchTodaysMenu()
            |> observeOn(QueueScheduler.mainQueueScheduler)
            |> on(error: { _ in
                // TODO: Make this more declarative.
                self.hideMenu.put(true)
            })
            |> ignoreError
    }
    |> observeOn(QueueScheduler.mainQueueScheduler)
```

Now this is cool! Let's go through it:

- Start with the signal producer that sends values to tell whether or not the view is active.
- Filter each time, using the above value, to make sure we only proceed when the view is active.
- `flatMap(FlattenStrategy.Latest)` (or `flattenMap` and `switchToLatest` in RAC 2.x) that into another signal producer that fetches the menu remotely.
- Make sure to return on the `mainQueueScheduler` as this will be presented in the UI.

# Showing the menu of today
Having the hardest part taking care of, the only thing we're missing now, is to actually present our content in the view. It seems like there's no extensions for UI elements yet, as Colin Eberhardt also mentions in [his blog post](http://blog.scottlogic.com/2015/05/15/mvvm-reactive-cocoa-3.html), but he was so kind to make [some for us](https://github.com/ColinEberhardt/ReactiveTwitterSearch/blob/master/ReactiveTwitterSearch/Util/UIKitExtensions.swift)

Having these extensions, it's a no-brainer to wire up our UI elements with our view model. Here's some examples:

```swift
self.headline.rac_hidden <~ self.viewModel.hideMenu
self.headline.rac_text <~ self.viewModel.headline
self.logo.rac_image <~ self.viewModel.logo
```

# That's a wrap

![We're done!](/assets/posts/2348745.gif)

That's it! I hope this post will help you get started with ReactiveCocoa 3 as it took me quite a while just to setup basic bindings. I did take some shortcuts and I consider the project as being experimental and as work in progress. Feel free to ping me at [@steffendsommer](https://twitter.com/steffendsommer) or leave a comment with any feedback or questions.

## Some other useful resources
ReactiveCocoa 3.0 is, at the time of writing, still very new and because of that, the resources are limited. I also know that this post is jumping straight into the bits and pieces of the app instead of explaining the concepts of ReactiveCocoa 3.0. If you're looking for other resources, here are some I used while creating this project:

- [ReactiveCocoa 3.0 changelog](https://github.com/ReactiveCocoa/ReactiveCocoa/blob/swift-development/CHANGELOG.md) (make sure to see the implementation, tests and issues as well)
- Colin Eberhardt's [ReactiveTwitterSearch](https://github.com/ColinEberhardt/ReactiveTwitterSearch)
- [This](http://nomothetis.svbtle.com/an-introduction-to-reactivecocoa) and [this](http://nomothetis.svbtle.com/reactivecocoa-ii-reacting-to-signals) post by Alexandros Salazar
- The [reactivecocoa-3](http://stackoverflow.com/questions/tagged/reactive-cocoa-3) tag on SO

