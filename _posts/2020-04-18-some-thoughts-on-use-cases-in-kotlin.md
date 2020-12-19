---
id: 137
title: Some thoughts on use cases in Kotlin
date: 2020-04-18T10:23:33+00:00
author: jordan_terry
layout: post
guid: http://jordanterry.co.uk/?p=137
permalink: /some-thoughts-on-use-cases-in-kotlin
categories:
  - Uncategorized
---
Originally published here on Medium: <https://medium.com/@jordanfterry/some-thoughts-on-use-cases-in-kotlin-6ac8021cbcf1>

Recently at the Guardian we’ve started to apply the use case pattern to our business logic. A high level overview of a use case is a class that will encapsulate a particular piece of business logic, or behaviour in your app. You may know of this as an interactor pattern as advocated for by <a rel="noreferrer noopener" href="https://blog.cleancoder.com/" target="_blank">Robert Martin</a> in <a rel="noreferrer noopener" href="https://www.amazon.com/Clean-Architecture-Craftsmans-Software-Structure/dp/0134494164" target="_blank">Clean Architecture</a>. They are easy to interpret and test, which will in turn increase both developer productivity and confidence in the quality of a team’s code.

I like to call our use cases “functional use cases”? Why? Well, we make use of <a rel="noreferrer noopener" href="https://kotlinlang.org/docs/reference/operator-overloading.html" target="_blank">operator overloading</a> to override the `invoke` function to make execution look like a function. Pretty simple really! Here is an example of how this might look:

<pre class="wp-block-preformatted">class FunctionalUseCase() {
   operator fun invoke() {
       // Do something snazzy here.
   }
}</pre>

And when we invoke it:

<pre class="wp-block-preformatted">val useCase = FunctionalUseCase()<br />useCase() // Not a function, but calling overloaded invoke function</pre>

It isn’t revolutionary but I like writing our use cases like this. Here are some reasons, and some other musings I have on the topic of use cases.

Overloading the invoke function is straightforward and flexible and allows you to make use of a great Kotlin feature. It is, as you might say, more idiomatic!

It is straightforward because you only have to implement the invoke function and you are good to go. Anyone with knowledge of operator overloading should be able to look at our code and know what is happening.

It is flexible as you can add parameters whatever parameters you like to the invoke function to suit the needs of the particular use case. I think this is great as we can provide any class dependencies as a constructor parameters and provide contextual information as function parameters.

One thing I’ve learned from use case “efforts” in the past is creating an opinionated use case such as one that uses RxJava to handle threading could be a mistake. It might look like this:

<pre class="wp-block-preformatted">abstract class SingleUseCase&lt;T&gt;(<br />    private val observeOn: Scheduler, <br />    private val subscribeOn: Scheduler<br />) {</pre>

<pre class="wp-block-preformatted">fun execute(): Single&lt;T&gt; {<br />        return Single<br />            .fromCallable { doTheUseCase() }<br />            .observeOn(observeOn)<br />            .subscribeOn(subscribeOn)<br />    }</pre>

<pre class="wp-block-preformatted">abstract fun doTheUseCase(): T</pre>

<pre class="wp-block-preformatted">}
</pre>

This could lead to some sneaky misdirection making it hard for developers to find usages of their implementation of the abstract class, as generically wrapping some behaviour in another type will require an abstract function to fill in that behaviour. A developer won’t be able to find all usages of their implementation as their implementation will always be used in the super class.

This is Rx specific, but you may have to implement multiple classes to handle `Observable`, `Flowable`, `Completable` or `Maybe`&nbsp;. This just adds a bit of extra complexity to your use cases.

Something I think about regularly is the single responsibility principle (I need more hobbies). It says aclass should only ever have a single reason to change. But in the above implementation, your use case can change if your business rules change or if you decide to stop using RxJava. It breaks SRP in a very subtle way!

Talking of developer experience, there is a particularly annoying gotcha with overloading the invoke function in Kotlin, it relates more to some behaviour in Android Studio/IntelliJ. But lets look at this class:

<pre class="wp-block-preformatted">class AViewModel(private val useCase: UseCase) {</pre>

<pre class="wp-block-preformatted">fun start() {<br />        useCase()<br />    }<br />}</pre>

If you wanted to go to the source of `useCase` you would be forgiven for thinking you could click on `useCase` within the `start` function but you would be wrong. You will actually be taken to the definition of the property. To be taken to the source you’ll have to carefully aim your cursor on the final bracket: `useCase(<strong>)</strong>`**.** This is very frustrating if you are trying to quickly navigate through some code!

This quickly turned into me complaining about some old code I have written and applauding some code I’m currently writing. I expect I’ll change my mind on this in the next year or so, but I hope some of these thoughts will be useful to someone!