---
id: 151
title: 'Slice don&#8217;t Splice'
date: 2020-07-06T08:06:54+00:00
author: jordan_terry
layout: post
permalink: /slice-dont-splice
categories:
  - Software Engineering
---
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/wp-content/uploads/2020/07/juja-han-l3KOZAMPBOU-unsplash-1024x683.jpg" alt="" class="wp-image-168" srcset="{{ site.baseurl }}/wp-content/uploads/2020/07/juja-han-l3KOZAMPBOU-unsplash-1024x683.jpg 1024w, {{ site.baseurl }}/wp-content/uploads/2020/07/juja-han-l3KOZAMPBOU-unsplash-300x200.jpg 300w, {{ site.baseurl }}/wp-content/uploads/2020/07/juja-han-l3KOZAMPBOU-unsplash-768x512.jpg 768w, {{ site.baseurl }}/wp-content/uploads/2020/07/juja-han-l3KOZAMPBOU-unsplash-1536x1024.jpg 1536w, {{ site.baseurl }}/wp-content/uploads/2020/07/juja-han-l3KOZAMPBOU-unsplash.jpg 1920w" sizes="(max-width: 1024px) 100vw, 1024px" /><figcaption>Photo by [Juja Han](https://unsplash.com/@juja_han?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/slice?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)</figcaption></figure> 

This weekend I&#8217;ve spent some time working on a side project written [TypeScript](https://www.typescriptlang.org/), I&#8217;ve never used it before so I&#8217;ve spent a lot of time referring to documentation and learning a lot. One thing stood out.

I had an array of data that I wanted to create a sub-list of elements starting from and index, `i`, to a range. You can do this by calling `array.slice`:

<blockquote class="wp-block-quote">
  <p>
    &#8220;Extracts a section of the array and returns the new array&#8221;
  </p>
  
  <cite>https://www.tutorialsteacher.com/typescript/typescript-array</cite>
</blockquote>

Typescript, or the underlying Javascript also has a function that adds or removes elements from an array. This is called `array.splice`:

<blockquote class="wp-block-quote">
  <p>
    Adds or removes elements from the array
  </p>
  
  <cite>https://www.tutorialsteacher.com/typescript/typescript-array</cite>
</blockquote>

I suppose I don&#8217;t need to go into detail about what caused me to write this blog post, but I have some lessons: 

  * Pay close detail to the functions you are writing or selecting from autocomplete.
  * Test every single piece of code you change, even if you think you are making a small change
  * Unit testing isn&#8217;t always enough to catch issues, especially when your unit is manipulating data being passed into it

I&#8217;d also like to call out the concept of immutability. This would have saved a stupid developer from a stupid mistake.