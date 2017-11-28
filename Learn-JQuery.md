### jQuery: The Basics

Getting started with jQuery can be easy or challenging, depending on your experience with JavaScript, HTML, CSS, and programming concepts in general. In addition to these articles, you can read about the [history of jQuery](https://jquery.org/history/) and the [licensing terms](https://jquery.org/license/) that apply to jQuery projects. You can also [make a donation](https://jquery.org/donate/) to help the [jQuery team](https://jquery.org/team/) continue to improve jQuery.

One important thing to know is that jQuery is just a __JavaScript library__. All the power of jQuery is accessed via JavaScript, so having a strong grasp of JavaScript is essential for understanding, structuring, and debugging your code. While working with jQuery regularly can, over time, improve your proficiency with JavaScript, it can be hard to get started writing jQuery without a working knowledge of JavaScript's built-in constructs and syntax. Therefore, if you're new to JavaScript, we recommend checking out the [JavaScript basics tutorial](https://developer.mozilla.org/en-US/Learn/Getting_started_with_the_web/JavaScript_basics) on the Mozilla Developer Network (MDN).

This is a basic tutorial, designed to help you get started using jQuery. If you don't have a test page setup yet, start by creating the following HTML page:

```
<!doctype html>
<html>
<head>
	<meta charset="utf-8">
	<title>Demo</title>
</head>
<body>
	<a href="http://jquery.com/">jQuery</a>
	<script src="jquery.js"></script>
	<script>

	// Your code goes here.

	</script>
</body>
</html>
```

The `src` attribute in the `<script>` element must point to a copy of jQuery. Download a copy of jQuery from the [Downloading jQuery](http://jquery.com/download/) page and store the `jquery.js` file in the same directory as your HTML file.

<div class="warning">**Note**: When you download jQuery, the file name may contain a version number, e.g., `jquery-x.y.z.js`. Make sure to either rename this file to `jquery.js` or update the `src` attribute of the `<script>` element to match the file name.</div>

### Launching Code on Document Ready

To ensure that their code runs after the browser finishes loading the document, many JavaScript programmers wrap their code in an `onload` function:

```
window.onload = function() {

	alert( "welcome" );

};
```

Unfortunately, the code doesn't run until all images are finished downloading, including banner ads. To run code as soon as the document is ready to be manipulated, jQuery has a statement known as the [ready event](http://api.jquery.com/ready/):

```

$( document ).ready(function() {

	// Your code here.

});
```

<div class="warning">**Note**: The jQuery library exposes its methods and properties via two properties of the <code>window</code> object called <code>jQuery</code> and <code>$</code>. <code>$</code> is simply an alias for <code>jQuery</code> and it's often employed because it's shorter and faster to write.</div>

For example, inside the `ready` event, you can add a click handler to the link:

```
$( document ).ready(function() {

	$( "a" ).click(function( event ) {

		alert( "Thanks for visiting!" );

	});

});
```

Copy the above jQuery code into your HTML file where it says `// Your code goes here`. Then, save your HTML file and reload the test page in your browser. Clicking the link should now first display an alert pop-up, then continue with the default behavior of navigating to http://jquery.com.

For `click` and most other [events](http://api.jquery.com/category/events/), you can prevent the default behavior by calling `event.preventDefault()` in the event handler:

```
$( document ).ready(function() {

	$( "a" ).click(function( event ) {

		alert( "As you can see, the link no longer took you to jquery.com" );

		event.preventDefault();

	});

});
```

Try replacing your first snippet of jQuery code, which you previously copied in to your HTML file, with the one above. Save the HTML file again and reload to try it out.

### Complete Example

The following example illustrates the click handling code discussed above, embedded directly in the HTML `<body>`. Note that in practice, it is usually better to place your code in a separate JS file and load it on the page with a `<script>` element's `src` attribute.

```
<!doctype html>
<html>
<head>
	<meta charset="utf-8">
	<title>Demo</title>
</head>
<body>
	<a href="http://jquery.com/">jQuery</a>
	<script src="jquery.js"></script>
	<script>

	$( document ).ready(function() {
		$( "a" ).click(function( event ) {
			alert( "The link will no longer take you to jquery.com" );
			event.preventDefault();
		});
	});

	</script>
</body>
</html>
```

### Adding and Removing an HTML Class

<div class="warning">**Important:** You must place the remaining jQuery examples inside the `ready` event so that your code executes when the document is ready to be worked on.</div>

Another common task is adding or removing a class.

First, add some style information into the `<head>` of the document, like this:

```
<style>
a.test {
	font-weight: bold;
}
</style>
```

Next, add the [.addClass()](http://api.jquery.com/addClass/) call to the script:

```
$( "a" ).addClass( "test" );
```

All `<a>` elements are now bold.

To remove an existing class, use [.removeClass()](http://api.jquery.com/removeClass/):

```
$( "a" ).removeClass( "test" );
```

### Special Effects

jQuery also provides some handy [effects](http://api.jquery.com/category/effects/) to help you make your web sites stand out. For example, if you create a click handler of:

```
$( "a" ).click(function( event ) {

	event.preventDefault();

	$( this ).hide( "slow" );

});
```

Then the link slowly disappears when clicked.

## Callbacks and Functions

Unlike many other programming languages, JavaScript enables you to freely pass functions around to be executed at a later time. A *callback* is a function that is passed as an argument to another function and is executed after its parent function has completed. Callbacks are special because they patiently wait to execute until their parent finishes. Meanwhile, the browser can be executing other functions or doing all sorts of other work.

To use callbacks, it is important to know how to pass them into their parent function.

### Callback *without* Arguments

If a callback has no arguments, you can pass it in like this:

```
$.get( "myhtmlpage.html", myCallBack );
```

When [$.get()](http://api.jquery.com/jQuery.get/) finishes getting the page `myhtmlpage.html`, it executes the `myCallBack()` function.

* **Note:** The second parameter here is simply the function name (but *not* as a string, and without parentheses).

### Callback *with* Arguments

Executing callbacks with arguments can be tricky.

#### Wrong

This code example will ***not*** work:

```
$.get( "myhtmlpage.html", myCallBack( param1, param2 ) );
```

The reason this fails is that the code executes `myCallBack( param1, param2 )` immediately and then passes `myCallBack()`'s *return value* as the second parameter to `$.get()`. We actually want to pass the function `myCallBack()`, not `myCallBack( param1, param2 )`'s return value (which might or might not be a function). So, how to pass in `myCallBack()` *and* include its arguments?

#### Right

To defer executing `myCallBack()` with its parameters, you can use an anonymous function as a wrapper. Note the use of `function() {`. The anonymous function does exactly one thing: calls `myCallBack()`, with the values of `param1` and `param2`.

```
$.get( "myhtmlpage.html", function() {

	myCallBack( param1, param2 );

});
```

When `$.get()` finishes getting the page `myhtmlpage.html`, it executes the anonymous function, which executes `myCallBack( param1, param2 )`.


While we hope to cover most jQuery-related topics on this site, you may need additional or more immediate support. The following resources can prove useful.

### Official Forums

http://forum.jquery.com/

There are many subforums where you can discuss jQuery, ask questions, talk about JavaScript, or announce your plugins.

* [Getting Started](http://forum.jquery.com/getting-started)
	* This is the best place to post if you are brand new to jQuery and JavaScript.
* [Using jQuery](http://forum.jquery.com/using-jquery)
	* This is the best place to post if you have general questions or concerns.
	* If you've built a site that uses jQuery, or would like to announce a new plugin, this is the place to do it.
* [Using jQuery Plugins](http://forum.jquery.com/using-jquery-plugins)
	* If you are a plugin author or user and you wish to discuss specific plugins, plugin bugs, new features, or new plugins.
* [Using jQuery UI](http://forum.jquery.com/using-jquery-ui)
	* This is the place to discuss use of [jQuery UI](http://jqueryui.com/) Interactions, Widgets, and Effects
* [jQuery Mobile](http://forum.jquery.com/jquery-mobile)
	* This is the place to discuss jQuery Mobile.
* [Developing jQuery Core](http://forum.jquery.com/developing-jquery-core)
	* This forum centers around development of the jQuery library itself.
	* Post here if you have questions about certain bugs, development with jQuery, features, or anything in the bug tracker or Git.
* [Developing jQuery Plugins](http://forum.jquery.com/developing-jquery-plugins)
	* This forum covers development of jQuery plugins.
* [Developing jQuery UI](http://forum.jquery.com/developing-jquery-ui)
	* This is the place to discuss development of [jQuery UI](http://jqueryui.com/) itself – including bugs, new plugins, and how you can help.
	* All jQuery UI svn commits are posted to this list to facilitate feedback, discussion, and review.
	* Also note that a lot of the development and planning of jQuery UI takes place on the [jQuery UI Development and Planning Wiki](http://wiki.jqueryui.com/).
* [Developing jQuery Mobile](http://forum.jquery.com/developing-jquery-mobile)
	* This forum covers issues related to the development of jQuery Mobile.
* [QUnit and Testing](http://forum.jquery.com/qunit-and-testing)
	* This is the place to discuss JavaScript testing in general and QUnit in particular

At the bottom of each of the forums is an RSS feed you can subscribe to.

To ensure that you'll get a useful answer in no time, please consider the following advice:

* Ensure your markup is valid.
* Use Firebug/Developer Tools to see if you have an exception.
* Use Firebug/Developer Tools to inspect the HTML classes, CSS, etc.
* Try expected resulting HTML and CSS without JavaScript/jQuery and see if the problem could be isolated to those two.
* Reduce to a minimal test case (keep removing things until the problem goes away, etc.)
* Provide that test case as part of your mail. Either upload it somewhere or post it on [jsbin.com](http://jsbin.com/).

In general, keep your question short and focused and provide only essential details – others can be added when required.

### Mailing List Archives

The mailing lists existed before the forums were created, and were closed in early 2010.

There are two different ways of browsing the mailing list archives.

1. The official mailing list archives can be found here:
	* [jQuery General Discussion Archives](http://groups.google.com/group/jquery-en)
		* [jQuery Dev List Archives](http://groups.google.com/group/jquery-dev)
		* [jQuery UI General Discussion Archives](http://groups.google.com/group/jquery-ui)
	* [jQuery UI Dev List Archives](http://groups.google.com/group/jquery-ui-dev)
	* [jQuery Plugins List Archives](http://groups.google.com/group/jquery-plugins)

2. Also, an interactive, browsable version of the General Discussion mailing list can be found on [Nabble](http://jquery.10927.n7.nabble.com/jQuery-General-Discussion-f3.html) (a forum-like mailing list mirror).

### Chat / IRC Channel

jQuery also has a very active IRC channel, `#jquery`, hosted by [freenode](http://freenode.net/).

The IRC Channel is best if you need quick help with any of the following:

* JavaScript
* jQuery syntax
* Problem solving
* Strange bugs

If your problem is more in-depth, we may ask you to post to the mailing list, or the bug tracker, so that we can help you in a more-suitable environment.

#### Connect info:

**Server:** irc.freenode.net

**Room:** `#jquery`

You can also connect at http://webchat.freenode.net/?channels=#jquery.

If you wish to post code snippets to the channel, you should use a paste site, like [jsfiddle.net](http://jsfiddle.net/) or [jsbin.com](http://jsbin.com/).

Additional info regarding jQuery's use of IRC can be found on [irc.jquery.org](http://irc.jquery.org).

### StackOverflow

There is an active and well-informed support community at [StackOverflow](http://stackoverflow.com/questions/tagged/jquery). You can likely find an answer for whatever issue you're experiencing. If your question isn't addressed, you can ask a new question and often receive a quick response.
