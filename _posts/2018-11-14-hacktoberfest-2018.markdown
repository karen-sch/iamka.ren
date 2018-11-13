---
layout: post
title:  "Hacktoberfest 2018"
date:   2018-11-14 08:00:00 +0100
categories: blog
tags: en hacktoberfest kotlin github godot open-source
---
Hacktoberfest is an annual event organized by GitLab and Digital Ocean. It's intended to lower the barrier for open source contributions: If you sign up and send five pull requests in the month of October, you can get a limited t-shirt for free. Repo maintainers can participate by tagging issues with the Hacktoberfest label.

While the real Oktoberfest is rather terrifying to me - drunk people in Lederhosen, no thanks - I'm always down for motivating myself to do stuff through gamification.

Open source is something I would like to take part in in theory, but in practice I'm currently not a regular contributor to any open source code bases. So I decided to do Hacktoberfest for the first time this year, because, why not?

![Hacktoberfest.]({{ "/assets/hacktoberfest.png"}})
*<center>Spoiler alert: I made it! Just in time 😊</center>*

The first question was which projects I could reasonably make a contribution to: I had set myself the goal to do "real" pull requests to actual projects that are not just for PR spam, but the whole thing was still supposed to be fun.

There are different labels in GitHub that can help you to find suitable issues to become a first time contributor: The aforementioned [Hacktoberfest](https://github.com/search?utf8=%E2%9C%93&q=label%3Ahacktoberfest&type=Issues&ref=advsearch&l=&l=), or, all year round: ["help wanted"](https://github.com/search?utf8=%E2%9C%93&q=label%3A%22help+wanted%22&type=Issues&ref=advsearch&l=&l=) or ["good first issue"](https://github.com/search?q=label%3A%22good+first+issue%22&type=Issues).

## Start with what you know

I searched through all those labels, but in the end I deciced to look at projects I was already familiar with instead of something random from GitHub.  
The idea being: If you're using something (a tool, library, framework, ...) on a daily basis, you're already aware of how it works and what it's strengths and weaknesses are. Maybe you have even already patched your local version of this tool somehow. Ergo: excellent PR fodder!

For me, that was the case with the [Godot game engine](https://github.com/godotengine/godot). Godot is used in a current work project, and we had indeed modified it for our use case.
I opened a PR to fix a problem we ran into locally. I also contributed to the documentation by explaining a feature that my colleague Patrick had added to Godot earlier.

Documentation is a great topic for initial contributions in general: As a user of a tool or library, you might even have a better grasp of what the documentation lacks or is unclear about than the project maintainers, who already know everything inside out.

For another contribution, I corrected some typos and formatting in the README of a repo I had just read. These kind of things always low hanging fruit as well: Non-controversial and  helpful.

Another PR fell again in the "familiar stuff" category: I converted an editor theme I use for a wider range of editors, something I had wanted to do for a while.

## Kontributing to Kotlin

One of the cool things about the Kotlin programming language is that it's open source! So, I thought, it must be possible to contribute somehow.  
JetBrains has its own [issue tracker](https://youtrack.jetbrains.com/issues/KT) for Kotlin, separate from the GitHub repo. The main label to welcome outside contributions on issues is called ["Up for Grabs"](https://youtrack.jetbrains.com/issues/KT?q=tag:%20%7BUp%20For%20Grabs%7D).

I looked around for a bit and found [KT-20357](https://youtrack.jetbrains.com/issue/KT-20357), an issue specifically for those who want to work on Kotlin for the first time.
It's about adding samples to the standard library. Samples are small code snippets that demonstrate the usage of a standard library function. They are embedded as executable code into the documentation ([like here](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/chunked.html)).
The reader can also modify the samples and execute them again, so the documentation is interactive!

I decided to create samples for the `minBy` and `maxBy` functions.  
In the case of `maxBy`, my sample looks like this:

{% highlight kotlin %}
@Sample
fun maxBy() {
    val nameToAge = listOf("Alice" to 42, "Bob" to 28, "Carol" to 51)
    val oldestPerson = nameToAge.maxBy { it.second }
    assertPrints(oldestPerson, "(Carol, 51)")

    val emptyList = emptyList<Pair<String, Int>>()
    val emptyMax = emptyList.maxBy { it.second }
    assertPrints(emptyMax, "null")
}
{% endhighlight %}

Samples are implemented as unit tests behind the scenes (`Sample` is a `typealias` for `org.junit.Test`), but they are not supposed to test edge cases, just show how one would typically use a function.

Samples are created in the subfolders of `libraries/stdlib/samples/test/samples`.
A new sample then has to be referenced in the corresponding standard libary template.  

This is also how I learned how the Kotlin standard library is created in the first place: Because some of the extension methods exist for different types, they are not copied with slight modifications every time, but automatically generated instead. Which makes sense of course, but I had never really thought about it before. `minBy` and `maxBy` for example exist for Iterables, Sequences, object as well as primitive arrays, Maps, and CharSequences.

In a very meta way, the generation uses templates which are created using a Kotlin DSL (domain specific language). This DSL contains a `sample` method to add the sample reference to the method template. After I added the new samples, I had to re-generate the standard library, now containing the new sample references. If you want to learn more about the Kotlin standard libary generation, look [here](https://github.com/JetBrains/kotlin/tree/master/libraries/tools/kotlin-stdlib-gen).

If you want to contribute a sample to Kotlin, start with this [README](https://github.com/JetBrains/kotlin/blob/master/libraries/stdlib/samples/ReadMe.md).

I didn't just learn a lot by doing this, but I'm really happy to say that [my pull request](https://github.com/JetBrains/kotlin/pull/1948) has since been merged into Kotlin. 😊

## Summary

Here are some things that worked for me:

* Start with already familiar projects ❤️
* Issues marked specifically for newbies 👶
* Low hanging fruits: Typos, formatting, etc. 🍇
* Documentation! ℹ️
* Talk to the maintainers first for anything bigger! 💬
* Read all the things: READMEs, contribution guidelines, Code of Conduct... 📜
* If possible: Go to meetups in your area & work with others 🤝

I have to say that's it's really exciting in a fun way to open pull requests on new repos.  
I'll probably do it again in the future. 😬 (As a bonus, everyone who reviewed my changes was super nice so far.)
