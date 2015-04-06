---
layout: post
title: "Fast HTML rendering without UIWebView"
date: 2013-04-08 22:14
comments: true
categories:
- NSAttributedString
- iOS6

---

This project was created for [amrc](https://itunes.apple.com/au/app/amrc-reddit-client/id572391252?mt=8) as a means of rendering HTML rich text content.

The library is built on xml parser [hpple](https://github.com/topfunky/hpple). `htmpple` uses NSDictionary merging to produce an `NSAttributedString` that represents the given html. Only a subset of html tags are supported, they include those available to any rich text editor, layout such as `table` is too complex to render with just `NSAttributedString`.

Rendering non-hyperlink content is a peice of cake in iOS 6 due to the support added to `[UILabel attributedString]` . If the HTML contains links there's a bit of additional hacking required. I've included a subclass of `UITextView` to assist with this, although you may want to consider delving into `CoreText` aswell.

The helper method `htmlDataContainsLinks` is used to detect hyperlinks and mitigate the performance hit when using `UITextView`

Check out the project on [github.com/amleszk/htmpple](http://github.com/amleszk/htmpple)
