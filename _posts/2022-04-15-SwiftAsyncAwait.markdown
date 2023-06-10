---
layout: post
title: "Swift Concurrency I: Async/Await"
date:   2022-04-15 12:02:05 +0100
categories: coding
author: Javier Carrillo
permalink: /:categories/:day/:month/:year/:title.html
published: true
tags: coding
---
<h3 style="color: #403F3F">What's concurrency?</h3>
As you may know, concurrency is a very interesting tool when it comes, for instance, to build network calls. In that case we do not want to pause all the tasks that our app is running just because networks functions are running, mainly because that behaviour could freeze the user interface tasks that are updating the views and letting the user to keep flowing throught the app. So that's why it is better to perform all network functions as asynchronous ones.

When it comes to Concurrency in Swift we are in reallity talking about asynchronous and parallel tasks. Asynchronous tasks are those that can be suspended and resumed when needed in order to let other tasks with a higher level of priority to be runned, and parallel tasks are those that are able to be executed at the same time through a multicore processor.

<br>
<h3 style="color: #403F3F">Quick approach to Async/Await</h3>
The main example of an asynchronous function is a closure one (callback). These sort of functions are really useful, but sometimes, when we use a closure inside another closure inside another closure inside... it turns out that it is very difficult to follow the code and to figure out what the heck is the real sequence of the process. Let's see a quick example of this:

<style>.hljs-variable{color:#DABAFF;}.hljs-params{color:#ACF2E4;}.hljs-emphasis{font-style:italic;}.hljs-title{color:#6BDFFF;}.hljs-deletion{color:#DABAFF;}.hljs-meta{color:#B281EB;}.hljs-name{color:#DABAFF;}.hljs-type{color:#ACF2E4;}.hljs-symbol{color:#FF8170;}.hljs-regexp{color:#DABAFF;}.hljs-built_in{color: #B281EB;}.hljs-template-variable{color:#DABAFF;}.hljs-literal{color: #B281EB;}.hljs-section{color:#6BDFFF;}.hljs-strong{font-weight:bold;}.hljs-string{color:#FF8170;}.hljs-comment{color:#7F8C98;}.hljs-link{color:#DABAFF;}.hljs-attribute{color:#DABAFF;}.hljs-quote{color:#7F8C98;}.hljs-selector-id{color:#DABAFF;}.hljs-number{color: #D9C97C;}.hljs-tag{color:#DABAFF;}.hljs-addition{color:#FF8170;}.hljs-selector-class{color:#DABAFF;}.hljs-function{color:#6BDFFF;}.hljs{color:#E0E0E0;padding:0.5em;display:block;}.hljs-class{color:#6BDFFF;}.hljs-keyword{color:#FF7AB2;}.hljs-builtin-name{color: #B281EB;}.hljs-selector-tag{color:#FF7AB2;}.hljs-bullet{color:#FF8170;}</style>

<pre style="background-color: #292A30; border-radius:8px; border-top: 0px solid gray; border-left: 0px solid gray; border-right: 0px solid gray; border-bottom: 0px solid #DDDDDD"><code class="hljs" style="background:#292A30;border-radius:8px"><span class="hljs-keyword">import</span> UIKit

<span class="hljs-function"><span class="hljs-keyword">func</span> <span class="hljs-title">imageURLRequest</span><span class="hljs-params">(<span class="hljs-keyword">for</span> id: String)</span></span> -&gt; <span class="hljs-type">URLRequest</span> {
    <span class="hljs-keyword">let</span> url =<span class="hljs-attribute"> URL</span>(string: <span class="hljs-string">"url/"</span> + id)!
    <span class="hljs-keyword">let</span> request =<span class="hljs-attribute"> URLRequest</span>(url: url)
    <span class="hljs-comment">//URLRequest configuration</span>
    <span class="hljs-keyword">return</span> request
}

<span class="hljs-class"><span class="hljs-keyword">extension</span> <span class="hljs-title">UIImage</span> </span>{
    <span class="hljs-function"><span class="hljs-keyword">func</span> <span class="hljs-title">prepareImage</span><span class="hljs-params">(of size: CGSize, completion: @escaping <span class="hljs-params">(UIImage?)</span></span></span> -&gt; <span class="hljs-type">Void</span>) {
        
    }
}

<span class="hljs-class"><span class="hljs-keyword">enum</span> <span class="hljs-title">FetchImageError</span>: <span class="hljs-title">Error</span> </span>{
    <span class="hljs-keyword">case</span> badID
    <span class="hljs-keyword">case</span> badImage
}

<span class="hljs-function"><span class="hljs-keyword">func</span> <span class="hljs-title">fetchImage</span><span class="hljs-params">(<span class="hljs-keyword">for</span> id: String, completion: @escaping <span class="hljs-params">(Result&lt;UIImage, FetchImageError&gt;)</span></span></span> -&gt; <span class="hljs-type">Void</span>) {
    <span class="hljs-keyword">let</span> request =<span class="hljs-attribute"> imageURLRequest</span>(<span class="hljs-keyword">for</span>: id)
    <span class="hljs-keyword">let</span> task = <span class="hljs-type">URLSession</span>.<span class="hljs-attribute">shared</span>.<span class="hljs-attribute">dataTask</span>(with: request) { data, response, error <span class="hljs-keyword">in</span>
        <span class="hljs-keyword">if</span> <span class="hljs-keyword">let</span> error = error {
           <span class="hljs-attribute"> completion</span>(.<span class="hljs-attribute">failure</span>(.<span class="hljs-attribute">badID</span>))
        } <span class="hljs-keyword">else</span> <span class="hljs-keyword">if</span> (response <span class="hljs-keyword">as</span>? <span class="hljs-type">HTTPURLResponse</span>)?.<span class="hljs-attribute">statusCode</span> != <span class="hljs-number">200</span> {
           <span class="hljs-attribute"> completion</span>(.<span class="hljs-attribute">failure</span>(.<span class="hljs-attribute">badID</span>))
            <span class="hljs-keyword">return</span>
        } <span class="hljs-keyword">else</span> {
            <span class="hljs-keyword">guard</span> <span class="hljs-keyword">let</span> image =<span class="hljs-attribute"> UIImage</span>(data: data!) <span class="hljs-keyword">else</span> {
               <span class="hljs-attribute"> completion</span>(.<span class="hljs-attribute">failure</span>(.<span class="hljs-attribute">badImage</span>))
                <span class="hljs-keyword">return</span>
            }
            image.<span class="hljs-attribute">prepareImage</span>(of:<span class="hljs-attribute"> CGSize</span>(width: <span class="hljs-number">100</span>, height: <span class="hljs-number">100</span>)) { image <span class="hljs-keyword">in</span>
                <span class="hljs-keyword">guard</span> <span class="hljs-keyword">let</span> image = image <span class="hljs-keyword">else</span> {
                   <span class="hljs-attribute"> completion</span>(.<span class="hljs-attribute">failure</span>(.<span class="hljs-attribute">badImage</span>))
                    <span class="hljs-keyword">return</span>
                }
               <span class="hljs-attribute"> completion</span>(.<span class="hljs-attribute">success</span>(image))
            }
        }
    }
    task.<span class="hljs-attribute">resume</span>()
}

</code></pre>

In this example there are up to five `completion` executions...not very handy. So we need to figure out a new way to structure the code without losing any functionality in terms of asynchrony and error handling. And here it is when we came across with new `async/await` functions.

<style>.hljs-selector-id{color:#DABAFF;}.hljs-strong{font-weight:bold;}.hljs-symbol{color:#FF8170;}.hljs-quote{color:#7F8C98;}.hljs-keyword{color:#FF7AB2;}.hljs-deletion{color:#DABAFF;}.hljs-variable{color:#DABAFF;}.hljs-number{color: #D9C97C;}.hljs-title{color:#6BDFFF;}.hljs-section{color:#6BDFFF;}.hljs-tag{color:#DABAFF;}.hljs-meta{color:#B281EB;}.hljs-builtin-name{color: #B281EB;}.hljs-string{color:#FF8170;}.hljs{display:block;padding:0.5em;color:#E0E0E0;}.hljs-class{color:#6BDFFF;}.hljs-built_in{color: #B281EB;}.hljs-type{color:#ACF2E4;}.hljs-comment{color:#7F8C98;}.hljs-regexp{color:#DABAFF;}.hljs-literal{color: #B281EB;}.hljs-addition{color:#FF8170;}.hljs-selector-tag{color:#FF7AB2;}.hljs-link{color:#DABAFF;}.hljs-emphasis{font-style:italic;}.hljs-params{color:#ACF2E4;}.hljs-function{color:#6BDFFF;}.hljs-template-variable{color:#DABAFF;}.hljs-bullet{color:#FF8170;}.hljs-name{color:#DABAFF;}.hljs-attribute{color:#DABAFF;}.hljs-selector-class{color:#DABAFF;}</style>

<pre style="background-color: #292A30; border-radius:8px; border-top: 0px solid gray; border-left: 0px solid gray; border-right: 0px solid gray; border-bottom: 0px solid #DDDDDD"><code class="hljs" style="background:#292A30;border-radius:8px"><span class="hljs-class"><span class="hljs-keyword">extension</span> <span class="hljs-title">UIImage</span> </span>{
    <span class="hljs-function"><span class="hljs-keyword">func</span> <span class="hljs-title">prepareImageAsyncAwait</span><span class="hljs-params">()</span></span> <span class="hljs-keyword">async</span> -&gt; <span class="hljs-type">UIImage</span> {
        <span class="hljs-comment">//Whatever</span>
        <span class="hljs-keyword">return</span> <span class="hljs-type">UIImage</span>.<span class="hljs-attribute">init</span>()
    }
}

<span class="hljs-function"><span class="hljs-keyword">func</span> <span class="hljs-title">fetchImageAsyncAwait</span><span class="hljs-params">(<span class="hljs-keyword">for</span> id: String)</span></span> <span class="hljs-keyword">async</span> <span class="hljs-keyword">throws</span> -&gt; <span class="hljs-type">UIImage</span> {
    <span class="hljs-keyword">let</span> request =<span class="hljs-attribute"> imageURLRequest</span>(<span class="hljs-keyword">for</span>: id)
    <span class="hljs-keyword">let</span> (data, response) = <span class="hljs-keyword">try</span> <span class="hljs-keyword">await</span> <span class="hljs-type">URLSession</span>.<span class="hljs-attribute">shared</span>.<span class="hljs-attribute">data</span>(<span class="hljs-keyword">for</span>: request)
    <span class="hljs-keyword">guard</span> (response <span class="hljs-keyword">as</span>? <span class="hljs-type">HTTPURLResponse</span>)?.<span class="hljs-attribute">statusCode</span> == <span class="hljs-number">200</span> <span class="hljs-keyword">else</span> { <span class="hljs-keyword">throw</span> <span class="hljs-type">FetchImageError</span>.<span class="hljs-attribute">badID</span> }
    <span class="hljs-keyword">let</span> maybeImage =<span class="hljs-attribute"> UIImage</span>(data: data)
    <span class="hljs-keyword">guard</span> <span class="hljs-keyword">let</span> image = <span class="hljs-keyword">await</span> maybeImage?.<span class="hljs-attribute">prepareImageAsyncAwait</span>() <span class="hljs-keyword">else</span> { <span class="hljs-keyword">throw</span> <span class="hljs-type">FetchImageError</span>.<span class="hljs-attribute">badImage</span> }
    <span class="hljs-keyword">return</span> image
}</code></pre>

The first thing you could realize about is how simple it is, just a few lines of code. As you may noticed, when defining an asynchronous functions these are followed by `async`, and when instantiated them they are after an `await`. So with async/await we can bring two great advantages to our code:
- It is simple and easier to follow.
- Proper errors handling.

<br>
<h3 style="color: #403F3F">Properties and Sequences</h3>

As you may notice, the asynchronous function called `prepareImageAsyncAwait` is an extended method for UIImage class, and as I coded it...it does really nothing. That was just an example to show you how asynchronous functions must be defined and instantiated, but lets do that properly.

We are going to define `prepareImageAsyncAwait` as an extended property of UIImage class rather than a method. Is it possible to define asynchronous properties? The answer is: definitely yes, and async/await is very handy for this purpose.

<style>.hljs-quote{color:#7F8C98;}.hljs-string{color:#FF8170;}.hljs-link{color:#DABAFF;}.hljs-comment{color:#7F8C98;}.hljs-params{color:#ACF2E4;}.hljs-builtin-name{color: #B281EB;}.hljs-built_in{color: #B281EB;}.hljs-emphasis{font-style:italic;}.hljs-selector-tag{color:#FF7AB2;}.hljs-bullet{color:#FF8170;}.hljs-number{color: #D9C97C;}.hljs-attribute{color:#DABAFF;}.hljs-addition{color:#FF8170;}.hljs-meta{color:#B281EB;}.hljs-tag{color:#DABAFF;}.hljs-function{color:#6BDFFF;}.hljs-section{color:#6BDFFF;}.hljs{padding:0.5em;display:block;color:#E0E0E0;}.hljs-title{color:#6BDFFF;}.hljs-selector-class{color:#DABAFF;}.hljs-strong{font-weight:bold;}.hljs-symbol{color:#FF8170;}.hljs-template-variable{color:#DABAFF;}.hljs-class{color:#6BDFFF;}.hljs-selector-id{color:#DABAFF;}.hljs-literal{color: #B281EB;}.hljs-deletion{color:#DABAFF;}.hljs-type{color:#ACF2E4;}.hljs-keyword{color:#FF7AB2;}.hljs-name{color:#DABAFF;}.hljs-regexp{color:#DABAFF;}.hljs-variable{color:#DABAFF;}</style>

<pre style="background-color: #292A30; border-radius:8px; border-top: 0px solid gray; border-left: 0px solid gray; border-right: 0px solid gray; border-bottom: 0px solid #DDDDDD"><code class="hljs" style="background:#292A30;border-radius:8px"><span class="hljs-class"><span class="hljs-keyword">extension</span> <span class="hljs-title">UIImage</span> </span>{
    <span class="hljs-keyword">var</span> prepareImageAsyncAwait: <span class="hljs-type">UIImage?</span> {
        <span class="hljs-keyword">get</span> <span class="hljs-keyword">async</span> {
            <span class="hljs-keyword">let</span> size =<span class="hljs-attribute"> CGSize</span>(width: <span class="hljs-number">50</span>, height: <span class="hljs-number">50</span>)
            <span class="hljs-keyword">return</span> <span class="hljs-keyword">await</span> <span class="hljs-keyword">self</span>.<span class="hljs-attribute">byPreparingThumbnail</span>(ofSize: size)
        }
    }
}</code></pre>

When it comes to define asynchronous properties we have to bear in mind that only-read (get) ones are able to be asinchronous. `byPreparingThumbnail` method is an SDK built-in one and is asynchronous, that's why must be instantiated after `await`. In case the asynchronous property is able to throw any error we should define it as `get async throws`.

But what if we want to use an asynchronous function in a sequence? Well, in this case we need to use also `await`. Lets take a look to one of the most commonly used sequences: a for loop.

<style>.hljs-literal{color: #B281EB;}.hljs-type{color:#ACF2E4;}.hljs-params{color:#ACF2E4;}.hljs-variable{color:#DABAFF;}.hljs-quote{color:#7F8C98;}.hljs-meta{color:#B281EB;}.hljs-attribute{color:#DABAFF;}.hljs-section{color:#6BDFFF;}.hljs-number{color: #D9C97C;}.hljs-class{color:#6BDFFF;}.hljs-selector-tag{color:#FF7AB2;}.hljs-tag{color:#DABAFF;}.hljs-title{color:#6BDFFF;}.hljs{padding:0.5em;display:block;color:#E0E0E0;}.hljs-builtin-name{color: #B281EB;}.hljs-string{color:#FF8170;}.hljs-link{color:#DABAFF;}.hljs-regexp{color:#DABAFF;}.hljs-addition{color:#FF8170;}.hljs-built_in{color: #B281EB;}.hljs-selector-class{color:#DABAFF;}.hljs-symbol{color:#FF8170;}.hljs-emphasis{font-style:italic;}.hljs-keyword{color:#FF7AB2;}.hljs-deletion{color:#DABAFF;}.hljs-function{color:#6BDFFF;}.hljs-name{color:#DABAFF;}.hljs-bullet{color:#FF8170;}.hljs-comment{color:#7F8C98;}.hljs-template-variable{color:#DABAFF;}.hljs-selector-id{color:#DABAFF;}.hljs-strong{font-weight:bold;}</style>

<pre style="background-color: #292A30; border-radius:8px; border-top: 0px solid gray; border-left: 0px solid gray; border-right: 0px solid gray; border-bottom: 0px solid #DDDDDD"><code class="hljs" style="background:#292A30;border-radius:8px"><span class="hljs-keyword">for</span> <span class="hljs-keyword">await</span> id <span class="hljs-keyword">in</span> staticImageIDs.<span class="hljs-attribute">lines</span> {
    <span class="hljs-keyword">let</span> image = <span class="hljs-keyword">await</span><span class="hljs-attribute"> fetchImageAsyncAwait</span>(<span class="hljs-keyword">for</span>: id)
    imageArray.<span class="hljs-attribute">append</span>(image)
}</code></pre>

<br>
<h3 style="color: #403F3F">Summing Up</h3>

Lets point out a very important concepts we have to understand prior to start working with async/await executions:
- `async` enables a function to be suspended. **When a function suspends itself it suspends its callers too. That's why its callers must be `async` too.**
- `await` marks when the asynchronous function may be suspended. When the asynchronous function is completed, the executen resumes after the `await`.
- When an asynchronous execution is suspended the thread where it was running is not blocked, which means that other executions can be runned now, so the state of the app could change a lot since an asynchronous execution is suspended, and this situation could lead to a **data race issue**.

Thanks for reading :)

<table style="width: 100%; overflow: scroll; border-right: 0px solid gray; border-left: 0px solid gray">
    <tr style="border-right: 0px solid gray; border-left: 0px solid gray">
        <td style="width: 20%; border-top: 2px solid #DDDDDD; border-left: 0px solid gray; border-right: 0px solid gray; border-bottom: 0px solid gray; text-align: center; vertical-align: center; padding: 0px">
            <p style="color: #A8A8A8; font-size: 20px; margin: 0px 0px"><b>Written By</b></p>
        </td>
        <td style="border-top: 2px solid #DDDDDD; border-left: 0px solid gray; border-right: 0px solid gray; border-bottom: 0px solid gray; text-align: center; vertical-align: center; padding: 0px">
            <p style="color: #A8A8A8; font-size: 20px"><b></b></p>
        </td>
    </tr>
    <tr style="border-right: 0px solid gray; border-left: 0px solid gray">
        <td style="border-top: 0px solid gray; border-left: 0px solid gray; border-right: 0px solid gray; border-bottom: 2px solid #DDDDDD; color: gray; font-size: 20px; background-color: #FDFDFD; text-align: center; vertical-align: center; horizontal-align: center; padding: 5px">
        <img style="display: block; margin-left: auto; margin-right: auto; width: 100%; object-fit: contain" src="/assets/img/yo.png">
        </td>
        <td style="border-top: 0px solid gray; border-left: 0px solid gray; border-right: 0px solid gray; border-bottom: 2px solid #DDDDDD; background-color: #FDFDFD; text-align: left; vertical-align: center; padding: 10px">
            <p style="font-size: 26px; margin: 0px 0px"><b>Javi Carrillo</b></p>
            <p style="font-size: 18px">In the past I developed car's powertrain control software. Now I develope mobile apps for many purposes.</p>
        </td>
    </tr>
</table>




