---
layout: post
title: "RxPasscode"
date: 2016-04-18 22:36:00 -0400
---

This is a passcode UI component built using `RXSwift` and `LiveFrost`. I created this as a separate project to be able to test out the features quickly, for later integration into my app [Antenna](https://itunes.apple.com/us/app/antenna-client-for-reddit/id572391252?mt=8). This blog post outlines some of the design decisions and gotcha's encountered.

[https://github.com/amleszk/RxPasscode](https://github.com/amleszk/RxPasscode)

![Imgur](http://i.imgur.com/gQ1O2XB.png)

### Initial impressions on RxSwift

This is my first project to utilize `RxSwift`, so its usage of observables and streams is not very advanced. It takes a decent amount of thought to connect the streams in meaningful ways. I've learned the most from just looking at how other projects implement their observables. (I'll post a few links below) The key learning I wanted to share was that React has entry and exit points. One of the simplest entry points is the `Variable()`, this is a basic monad that wraps any piece of data. Once an object is an observable it becomes able to be manipulated easily.

To exit React world, you can use: `.subscribeNext { ..some code.. }`. But you want to avoid using this call, and instead use a Binding. Keeping the data in the react world means your code will be much more re-usable.

- **Tutorial:** [http://www.schibsted.pl/2016/03/search-in-frp/](http://www.schibsted.pl/2016/03/search-in-frp/) This is one of the best FRP tutorials I've found. The features are non-trivial and usage of react library is great. Watch the youtube videos also
- **Community:** [http://community.rxswift.org/](http://community.rxswift.org/) -- A list of helper libraries and examples. Again exploring others projects is my preferred way of learning, even though the designs of some components can be not the best
- **Official project** [https://github.com/ReactiveX/RxSwift](https://github.com/ReactiveX/RxSwift) This contains a few playground files which are worth checking out
- **Eidolon** [https://github.com/artsy/eidolon](https://github.com/artsy/eidolon) Complex project written in FRP, the devs were learning FRP as they wrote it, so don't assume they used best practices.

### The pain of using Swift and Objective C

Since I need this project to work with Objective C some of the code needed to change to be compatible. One such example is shared instance creation. I followed a blog post ([https://thatthinginswift.com/singletons/](https://thatthinginswift.com/singletons/)) which says the best way to create a shared instance is `class SomeManager { static let sharedInstance = SomeManager() }`. This is not compatible with Objective C code. Instead you need to do more work:

```swift
internal let passcodePresenterSharedInstance = PasscodePresenter()
class PasscodePresenter: NSObject {
    class var sharedInstance: PasscodePresenter {
        return passcodePresenterSharedInstance
    }
}
```

Also remember to always inherit from `NSObject` otherwise your classes will not be visible in Objective C. This brought me to the realization that using swift and objective C side-by-side will make your life **so much more complicated**. Consider dropping all objective C code somehow. Having seen projects of pure swift, and swift+objective-c, I would estimate that the compatibility makes the development time take 2-3 times longer, then having a pure swift project. If you have plans to move to swift, kill and/or quarantine your objective-c code, make it part of your plan.
