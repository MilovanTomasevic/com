---
layout: post
title: Xcode 6 error “Embedded binary's bundle identifier is not prefixed with the parent app's bundle identifier.”
image: https://developer.apple.com/news/images/og/xcode-og.png
description: >
  I've tried cleaning the build folder and rebuilding, no luck. The funny thing is the entire project got built for a few times, but after some code and UI changes it started failing. Even if I create new projects, the same thing happens after about 5-6 builds.
hide_description: true
hide_image: true
comments: true
last_modified_at: 2021-04-18
categories: [stackoverflow]
tags: [macos, ios, xcode, ios-app-extension]
---

Xcode 6 error: “Embedded binary's bundle identifier is not prefixed with the parent app's bundle identifier.”

{:.no_toc}
1. this unordered seed list will be replaced by toc as unordered list
{:toc}

## [Question](https://stackoverflow.com/questions/24045417/xcode-6-error-embedded-binarys-bundle-identifier-is-not-prefixed-with-the-par) by [chitza](https://stackoverflow.com/users/2073/chitza)

I'm trying to build an extension project and Xcode keeps throwing the error in subject.

![Xcode log error][1]

Needless to day, the extension's bundle id *is* prefixed with app's bundle id.

Product Name: `ro.chitza.TodayPics.$(TARGET_NAME:rfc1034identifier`

![Extension bundle id][2]

I've tried cleaning the build folder and rebuilding, no luck. The funny thing is the entire project got built for a few times, but after some code and UI changes it started failing. Even if I create new projects, the same thing happens after about 5-6 builds.

Did anyone else encounter this error? Any hints for a solution or workaround?


  [1]: http://i.stack.imgur.com/3d8Du.png
  [2]: http://i.stack.imgur.com/Q0HR4.png


## [Answer](https://stackoverflow.com/a/64874934/13155046) by [Milovan Tomašević](https://stackoverflow.com/users/13155046/milovan-tomašević)

For xCode v12 I solved that problem with one click:

TARGETS -> Build Phases -> Embed App Extensions

and check: `Copy only when installing`

[![enter image description here][3]][3]


  [3]: https://i.stack.imgur.com/TDfJs.png