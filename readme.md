# jQuery Plugins
## Screencasts
- [Part 1](https://youtu.be/ZkaP-uhnwQ0)
- [Part 2](https://youtu.be/dMwINJ00ahc)
- [Part 3](https://youtu.be/wLfWZviLvsQ)

## Learning Objectives

- Define what a jQuery plugin is
- Describe where to find existing jQuery Plugins (and how to install them)
- Demonstrate the basic structure of a jQuery Plugin
- Write your own jQuery Plugin


## Framing (10 min / 10 min total)
jQuery plugins are an extension of jQuery functionality. The idea of plugins is that you could build most of these on your own, but someone else already has! Many of the plugins that are out there are functionalities you're capable of building yourself. Only, now you don't have too. You can just use other people's stuff, it's great. As far as programming goes, we're not learning anything radically new.

**What are jQuery Plugins?**

Officially, plugins are "simply a new method that we use to extend jQuery's prototype object."  In practice, they enable to us to extend jQuery's functionality, from adding simple methods to jQuery objects (think `$()` or stuff wrapped in cash) to the [jQuery UI plugin](http://jqueryui.com) that is maintained by the jQuery team.

Think about the `.hide()` method in jquery. Under the hood its changing some css property using javascript. Much in the same way that jquery abstracts functionality from javascript, we can leverage javascript/jQuery to build our own jquery methods. This opens up a bunch of doors.

So you can see, they can be simple or rather complex.

But, you may be asking yourselves, "Why do we care?" <pause>

You tell me.  Check out these demos.

- [tablesorter](http://tablesorter.com/docs/#Demo)
- [isotope](http://codepen.io/desandro/full/nFrte)

Q. Why do we care?
---

> A. Encapsulation of really cool functionality, so we can reuse and share. We stand on the shoulders of giants. There's no need to recreate the wheel if it already exists.

## Basic anatomy of a jQuery Plugin?

What can we expect from jQuery plugins?  How do we use them?

1. Where do we find jQuery Plugins?<br>
<summary>
<details>
Officially a large list of Plugins have moved to: https://www.npmjs.com/browse/keyword/jquery-plugin
</details>
</summary>

2. What is the basic anatomy of a jQuery Plugin?<br>
<summary>
<details>

Basic Anatomy?
- Initialize with:
 - a single function called an IIFE
 - pass options
- Style guidelines ask that you add only one function per plugin.
</details>
</summary>

### An Aside - IIFE's (5 min / 15 min total)

An Immediately Invoked Function Expression (IIFE), is exactly what it sounds like... a function that is invoked immediately.

```js
(function(){
  // add some code here,
  //   including other functions.

})() // and then invoke it immediately
```

See those trailing parens?  We define an anonymous function and immediately invoke it.

Why would we do that?  Read
- [jQuery Best Practices](http://gregfranko.com/blog/jquery-best-practices/)
- [I love my IIFE](http://gregfranko.com/blog/i-love-my-iife/)

Q. Why do we use an IIFE?
---

> A.
- to locally scope jQuery.  
- To use the $ without fear of corruption from another library.
---


## We do - Fixed Content plugin

Let's actually implement a jquery plugin together.

The plugin we'll be using is [FixedContent.js](https://github.com/jeremychurch/FixedContent.js)

### Fixed Content Source (5min / 20min total)

The first thing I want to do is take a look at this source code so we can get a better understanding of plugins. The source code can be found [here](https://github.com/jeremychurch/FixedContent.js/blob/master/jquery.fixedcontent.js)

It's a little scary, but let's look at some parts we can identify

```javascript
(function($) {
  // this is just using regular jquery to basically say if there is something with a class of js_fixedcontent, do something
  if ($('.js_fixedcontent').length > 0) {
    // this is where the jquery prototype is extended to now include fixedcontent,
    // just like how you can call .hide on a jquery object, you can now call .fixedcontent
    $.fn.fixedcontent = function(options){
      // check out the source for the actual function def.
    };

    // calling the method defined above on anything with class js_fixed_content
    $('.js_fixedcontent').fixedcontent();

  }

}(jQuery));
```

> One thing we can note is this is an immediately invoked function expression. Where else have we seen this? (ST - WG). We need to use an IIFE in order for this plugin to work well with jQuery and other plugins. When we put all of this code into an IIFE, we need to pass the function `jQuery` and name the parameter `$`. If you'd like to know more about this, check this [link](https://learn.jquery.com/plugins/basic-plugin-creation/#protecting-the-alias-and-adding-scope) out.

### I Do: Implementation (10min / 30min total)
Let's create some folder and some files we'll need for this application in the terminal:

```bash
mkdir fixedcontent_demo
cd fixedcontent_demo
touch index.html
touch script.js
touch fixedContent.js
touch styles.css
```

Copy and paste the content from [here](https://raw.githubusercontent.com/jeremychurch/FixedContent.js/master/jquery.fixedcontent.js) into `fixedContent.js`

Use the content found in this [gist](https://gist.github.com/andrewsunglaekim/27d439df8ab05b6b176c) as the dummy html content for the `index.html`:

If we open up the index page we can see that the h1 remains at the top regardless of our scrolling!

Because of `$('.js_fixedcontent').fixedcontent();` in `fixedContent.js` we don't even have to call it in our script file. You can see inside the documentation that you can customize some of the default values. You might have something like this in your `script.js`:

```js
$(".js_fixedcontent").fixedcontent({
  marginTop: 24,
  minWidth: 767,
  zIndex: 500
});
```

## Your Own Plugin - You do (20min / 50min total)

Let's try and understand plugins a little bit deeper by creating our own custom made jQuery plugin. We'll be modeling our plugin after the one in these [jQuery docs](https://learn.jquery.com/plugins/basic-plugin-creation/#basic-plugin-authoring)

Lets start by creating all the files we'll need:

```bash
mkdir greenify
cd greenify
touch index.html
touch script.js
touch greenify.js
```

> Here we're just making a regular old `index.html` and `script.js`. Initially we'll be defining most of our plugins functionality in `script.js` and then we'll abstract the plugin portion to `greenify.js`. `greenify.js` will eventually be the jQuery plugin we'll be building.

In `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Greenify Demo</title>
</head>
<body>
<!-- some headers we'll be using as click event listeners -->
<h1>Make a div</h1>
<h2>Make em green</h2>
<h3>Add a green div</h3>
<!-- including jquery and script files we'll need -->
<script src='https://code.jquery.com/jquery-2.1.4.min.js'></script>
<script src='greenify.js'></script>
<script src='script.js'></script>
</body>
</html>
```

We'll be adding different bits of functionality in `script.js` incrementally throughout the next bit.

First add the following code to `script.js`

```js
$(document).ready(function(){
  $("h1").on("click", function(){
    $("body").append($("<div>jQuery plugins are cool!</div>"))
  })
})
```

We haven't added anything new just yet, all we've done is added an event listener to an `h1` that adds a `div` to the `body`. Let's now define our jQuery plugin and use it in a click event on the `h2`.

In `script.js`:

```js
$(document).ready(function(){
  (function($){
    $.fn.greenify = function(){
      this.css("color", "green")
      return this
    }
  })(jQuery);

  $("h1").on("click", function(){
    $("body").append($("<div>jQuery plugins are cool!</div>"))
  })

  $("h2").on("click", function(){
    $("div").greenify()
  })
})
```

If we click the `h2`, we can see all the `div`'s generated by the `h1` click event are now green.

> Here we are using `$.fn` as a prototype for the jQuery object such that we can extend the prototypes functionality. Also note that the syntax of the IIFE is very similar to the `fixedContent` plugin we utilized earlier.

### Common mistake

If you forget to wrap your IIFE in parens, you will get `SyntaxError: Function statements must have a name.`

## Leverage your existing plugin

Make a plugin... using a plugin!

Add this to our `script.js`:

```js
$(document).ready(function(){
  (function($){
    $.fn.greenify = function(){
      this.css("color", "green")
      return this
    }

    // added this function that uses the greenify function we defined above
    $.fn.addGreenDiv = function(){
      var newDiv = $("<div>jQuery plugins are really really cool!</div>").greenify()
      this.append(newDiv)
      return this
    }
  })(jQuery);

  $("h1").on("click", function(){
    $("body").append($("<div>jQuery plugins are cool!</div>"))
  })

  $("h2").on("click", function(){
    $("div").greenify()
  })

  $("h3").on("click", function(){
    $("body").addGreenDiv()
  })
})
```

Click on that `h3` and you should see a green `jQuery plugins are really really cool!`

## Don't break the chain!

Something we haven't talked about is the fact that we return `this` in our plugin definition. If we try out our code as it stands, it works great. Try removing

```js
return this
```

You'll see that that the `h1` and `h2` functionalities are still working great, but the `h3` no longer works. Why do you think this is? (ST - WG)

In our `addGreenDiv` function we call greenify on a hard coded div. If `greenify` doesn't have an explicit return(ie. missing `return this`), it returns `undefined` it appends something that is `undefined`.

jQuery functions, by convention, are chainable.  We should remember to return the jQuery object so other methods can be chained.

```js
// allow jQuery chaining
return this;
```

## Make it a plugin

Our `script.js` is getting to cluttery. And our plugin definition is getting mixed in with our logic. Let's abstract this functionality into a plugin we can use in the future at any time!

Remember that `greenify.js` file? What do you think goes in there?

In `greenify.js`:

```js
(function($){
  $.fn.greenify = function(){
    this.css("color", "green")
    return this
  }

  $.fn.addGreenDiv = function(){
    this.append($("<div>jQuery plugins are really really cool!</div>").greenify())
    return this
  }
})(jQuery);
```

> make sure to get rid of any duplicate code in your `script.js` and require `greenify.js` in your `index.html`

- http://tutorialzine.com/2015/04/our-favorite-jquery-plugins-and-libraries-for-spring-2015/
- http://www.creativebloq.com/jquery/top-jquery-plugins-6133175
- http://designshack.net/articles/javascript/40-awesome-jquery-plugins-you-need-to-check-out/


## Additional Resources

- http://www.sitepoint.com/how-to-develop-a-jquery-plugin/
- http://blog.npmjs.org/post/111475741445/publishing-your-jquery-plugin-to-npm-the-quick
- http://www.jquery-tutorial.net/introduction/method-chaining/
