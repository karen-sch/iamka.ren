---
layout: post
title:  "Droidcon Berlin 2018"
date:   2018-07-10 10:00:00 +0200
categories: blog
tags: en droidcon android flutter kotlin conference
---
Droidcon Berlin 2018 － the conference's 10th(!) anniversary － happened from June 25th to 27th. For me, it was only my second time there. I was lucky to be able to attend with two of my lovely colleagues (Thanks, [Marko](https://twitter.com/markonussbaum) 😊).  
This post is a summary of things that stood out to me about these three days in Androidland.


## 💩⚡️🔪🔥☠️-platform

Is cross-platform not a dirty word anymore?  
It was certainly a popular topic this year. There was a cross-platform panel on community day starring Miriam Busch, Kevin Galligan, Said Tahsin Dane, Andy Dyer and Eugenio Marletti. Google's cross-platform framework Flutter was present everywhere. And of course there was Jake Wharton's keynote titled "Blurring the Line Between Native and Web".

### What even is cross-platform?

An important point from the panel:  
While many people might still have a very negative initial reaction  to the term, it makes sense to clarify what you mean when you talk about cross-platform.  

Cross-platform **UI**?  
That invokes dark memories of first generation cross-platform solutions like PhoneGap/Cordova, which render HTML in a WebView, ugly and slow compared to native apps, with UI that matches neither platform's convention.

Sharing the general behind-the-UI **architecture** of the app seems useful though. (Uber's [RIBs project](https://github.com/uber/RIBs) was mentioned as a solution for this.)  
And hardly anyone would dispute that not having to write UI-independent **business logic** twice is a good thing. Especially when your app has [An Algorithm™](https://youtu.be/DctKvZOU56I?t=5m29s) at its center.
So, there's different layers to cross-platform.

Another problem with cross-platform: Unrealistic expectations. The panel participants agreed that there is no 100% "write once, run anywhere" solution, especially if you want to respect platform specific UX.
Writing cross-platform code never means that you just have to support one platform, it means that you have to support at least three: Android, iOS and the cross-platform solution itself. (Of course [Airbnb's React Native article series](https://medium.com/airbnb-engineering/react-native-at-airbnb-f95aa460be1c) about this very issue was also mentioned.)

### Fluttering toward a better future?

And yet, there is hope: When JS/HTML/WebView solutions like PhoneGap can be called the **first generation** of cross-platform, React Native might be the **second**. What you see on the screen are the original platform views, but the logic to drive them in written in JavaScript, with a bridge in between.

Flutter, the **third generation** new-kid-on-the-block works differently:
Each Flutter app ships Flutter's own Skia-based rendering engine. It's kind of like a game engine, only the views you see look like the platform views you are used to.

There is a wide range of Flutter widgets available for Android: Material Design even has a more comprehensive implementation on the Flutter side.
For iOS, there are so-called Cupertino widgets which are made to look iOS-platform-style.
Flutter is a Google project that is currently quite hyped among Android developers who are fed up with all those small quirks and big weaknesses of Android's platform views. (For instance, there was also a 40 minute [session](https://www.de.droidcon.com/Sessions/THE-CURIOUS-CASE-OF-ANDROID-BUTTON) by Nicola Cortini about styling secondary buttons in a different color.)

The promise on the horizon:

![You never have to say no to your designer any more.]({{ "/assets/designer.png"}})

*"You never have to say no to your designer any more."* (Quoted by Miriam Busch in her [presentation](https://speakerdeck.com/miriambusch/should-we-flutter?slide=16), originally by Eugenio Marletti, [probably](https://twitter.com/miphoni/status/1016751149112754176).)

A point that came up in Miriam Busch's talk "Should we flutter?": It's doubtful that there is the same allure from an iOS dev perspective. You would have to trust a competitor's team to ship new and updated imitation widgets for every new iOS version. And you have to hope that Apple doesn't mind these imposters too much.
Also, maybe a bitter pill for both iOS and Android devs: You have to write your apps in Dart instead of Swift and Kotlin that your teams just learned (to love?).

If you are unsure about Flutter, Miriam prepared a retro magazine style quiz to find out if it's right for your team and project! Take it [here](https://speakerdeck.com/miriambusch/should-we-flutter).

### App vs. Web

There is still the original cross-platform solution, build on open standards: the web. [In his keynote](https://speakerdeck.com/jakewharton/blurring-the-line-between-native-and-web-droidcon-de-2018), Jake Wharton outlined different ways that Android apps and web sites have gotten closer to each other in the past years:

Instant Apps are lightweight apps that are installed on the fly by clicking special URLs during web browsing. Similarly, the new app bundle distribution format initially only installs parts of the app that are relevant in the moment. When the conditions change, for example the user chooses a different language, the new string files are downloaded on the fly.  
On the other end of the spectrum, Android makes it possible for web sites to get their own icon on the home screen and function just like an app by packaging them in a so-called WebAPK.  
So the line between "browsing the web" and "explicitly downloading an app from the PlayStore" is already significantly blurred.

The web itself now has standards for all those various features previously reserved for native apps, even "low level" ones: Camera, microphone, location, hardware, even Bluetooth.  
Important key concepts for "app-like" web sites are [Progressive Web Apps](https://en.wikipedia.org/wiki/Progressive_Web_Apps) and [WebAssembly](https://webassembly.org/).

It's going to be interesting to see this line between native and web disappear further in the future.

### Kotlin Native / multiplatform

Another solution for code sharing, particularly of business logic, has of course always been C++, at least for sharing between Android and iOS. But: writing correct C++ code is hard and it might not always be the right tool for the job.  
Luckily, there is Kotlin multiplatform now. Kotlin can of course target the JVM and be compiled to Java byte code. But it can also be transpiled to JavaScript and Kotlin Native creates native binaries for various platforms. This native code is interoperable with Objective-C on macOS and iOS.  

In their session on Tuesday, Said Tahsin Dane and Tobias Heine from Novoda presented their solution for a three platform project on Android, iOS and the web. (There is a written form in this [blog post](https://blog.novoda.com/introduction-to-kotlin-multiplatform/).)  

In a Kotlin multiplatform project, there are platform modules for each platform, and a shared common module. It is possible to define `expect`ed declarations without implementations in the common module and then provide the implementations in the platform modules with the `actual` modifier.  
An example stolen [from the official docs](https://kotlinlang.org/docs/reference/multiplatform.html#platform-specific-declarations):

#### Common module
{% highlight kotlin %}
expect class Foo(bar: String) {
    fun frob()
}

fun main(args: Array<String>) {
    Foo("Hello").frob()
}
{% endhighlight %}

#### JVM implementation
{% highlight kotlin %}
actual class Foo actual constructor(val bar: String) {
    actual fun frob() {
        println("Frobbing the $bar")
    }
}
{% endhighlight %}

Two caveats for Kotlin multiplatform: It's currently still an experimental feature that might change quite a bit in the future.  Also, when you have a common module you can't use any Java classes in it. That might seem obvious, but coming from Android it's something you have to keep in mind.


## The meta stuff: insecurity, shame and internalization

I dislike the term *"soft"* for talks that focus on what it means and feels like to be a developer in general (instead of one particular technical thing).
Facing things like shame and impostor syndrome is extremely hard. To talk about these issues is especially meaningful in a five track conference with a constant stream of potentially important things.  
In the Android community, it sometimes seems like there is a new framework, library or architecture pattern every week.  It's impossible to keep up with 100% of it. And no matter how much you already read up on, at a developer conference like this, there's always someone who knows more.

This is why Anastasia López' talk ["The Authentic Developer"](https://speakerdeck.com/anastasialopez/authentic-developer) was probably my favorite talk of Droidcon Berlin. In it, she presented different strategies to deal with shame and treating oneself with kindness. Vulnerability itself is not a bad thing, she says, because it's important for growth and authenticity. She talked about how shame often has to do with internalized expectations of others and societal ideals.

What particularly stood out to me was her mentioning of survivorship bias. It had heard about it before, but in my head I had more of a caricature version of it:  that cliched obnoxious startup dude who had a lot of luck and privilege and now gives lectures about how everybody should do exactly what he did to become just as rich and successful.

But it's not just that guy, right? It's also the small stuff, and I've definitely perpetuated some of it before. Like, how learning to code it's just very hard right, and maybe it just has to hurt? And that other people must go through the same periods of feeling overwhelmed and alone in this, because I did. And I survived, and now I'm better, so that's just the way it goes, right? But actually, that's bulls**t.

I'm still working on this, but I definitely try to keep my survivorship bias in mind when I talk to people who are newer to coding. Because it shouldn't actually hurt! And it's so much easier when you're not alone. <3
