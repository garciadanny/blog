---
layout: post
title:  "Creating Custom AngularJS Directives"
date:   2014-11-09 20:15:28
categories: programming angularjs javascript angular google webdevelopment
---

Lately, I've been learning AngularJS in my spare time and have really been enjoying it. Angular has great documentation and there are plenty of books, tutorials, and blog posts to get you up and running quickly. As I mentioned in my last post, I'm rebuilding the admin portion of an old PHP app in AngualrJS.<!--more--> This app allows mobile app developers to create profiles on the site and connect with people who are looking to hire developers. In order to maintain the quality of the site and reduce spam, an administrator is responsible for reviewing newly created profiles before accepting them onto the site. The problem is that there is currently no way to filter between profiles that have already been accepted, declined, or not yet reviewed; There is only an index page that lists all of them. There are hundreds of profiles on the site currently, making reviewing those that have been newly created a difficult task.

## Using Angular's built in directives

Angular makes solving this problem a breeze with features like two-way data binding and filters. Here's a simple example on how you would go about doing this. Let's say we have a list of profiles attached the the scope in our controller:

app.js

<script src="https://gist.github.com/garciadanny/b5e238f4031648647c24.js"></script>

All we would then need to do is create an HTML select element and bind this element to the `status` property in our `scope`, thus dynamically creating a list of options based on the available `status` types in our list of profiles. We can accomplish this by using two directives built into angular; `ng-model` and `ng-options`.

index.html

<script src="https://gist.github.com/garciadanny/eca6150ff28f6620dbc8.js"></script>

Pay attention to what's going on in `ng-options`.

1). The first `profile.status` is saying "bind the status property of the profile object to the model we have named `status` in `ng-model="status"`"

2). The second `profile.status` will be evaluated and set as the label in the dropdown select menu.

3). `for profile in profiles` is simply iterating through all the profiles.

4). `| unique:'status'` is a filter which ensures that every option in the dropdown menu is unique so that we won't have two "Active" options since two of the profiles in our example have a status of "Active". This filter isn't built into angular and is provided by the [AngularUI](http://angular-ui.github.io/ui-utils/) suite. This is why it has been included as a dependency in our example app:

	var myApp = angular.module('myApp', ['ui.utils']);

5). Finally we can `filter` through all of the portfolios using our model `status`.

index.html

<script src="https://gist.github.com/garciadanny/9033ac72c8dd73a74ab8.js"></script>

Side Note:

The `true` option is a shorthand for strict filtering. Without it, Angular will filter based on a substring match, essentially making "Active" and "Inactive" identical since they both contain the word "active".

Here's a working example:

<iframe width="100%" height="300" src="http://jsfiddle.net/danny__garcia/cdk3sys9/embedded/result,html,js,resources" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Problem solved! In just a few lines of code we have solved our problem of not being able to filter through profiles. Now I could have simply commited these changes and moved on, but then I thought: "What if I wanted to filter based on other properties like city?".

## Creating a custom directive

We could simply create another HTML select element, create a `city` model and bind it to our scope, then add another filter to our list of portfolios.

index.html

<script src="https://gist.github.com/garciadanny/d9085933d47051b38586.js"></script>

The problem with this solution is that we are going to continue to repeat ourselves and write `<select>` elements for every filter option we want to provide. The more options we want to provide, the more duplication in our code. I therefore thought this would be a great opportunity to learn how to build custom directives that'll allow us to re-use code in our application.

Essentially, we'd like to be able to write something like this:

<script src="https://gist.github.com/garciadanny/47e69dfa2a0f2b7665be.js"></script>

And have it be converted to this:

<script src="https://gist.github.com/garciadanny/1b884e4bf15e40408bb2.js"></script>

There are several ways to create different types of directives; after doing some research online and digging through the docs, I couldn't find a convention for building this sort of directive so I did a little experimenting.

app.js

<script src="https://gist.github.com/garciadanny/98acda3462f3edf89c16.js"></script>

We have created a directive called selectFilter and have injected Angular's `$compile` service as one of our dependencies (we'll go over this later). When creating a directive, we want to create a factory function that returns an object with information about how the directive should behave. Let's break this down:

1) . `restrict: 'E'` means this is an `element` directive; allowing us to write the following HTML element:

`<select-filter></select-filter>`

Angular will convert our camelCase directive name `selectFilter` into `select-filter`.

2). There are several properties that can be supplied to the object a directive returns, `link` is one of them. `link` takes a function that defines the behavior of the directive and takes in the `scope` in context, the HTML `element` described above, and any `attributes` passed to it.

3). Within our `link` function, we are:

   * Grabbing the model ("profile") and attribute ("status"/"city") passed in the HTML and assigning them to variables.

   * We are also using an external library, [PluralizeJS](https://www.npmjs.org/package/pluralize), to pluralize the model name for us so that we don't have to worry about correctly pluralizing a model name. This becomes handy when the pluralized version of the model name doesn't end with an "s", such as "people". PluralizeJS will convert "person" into "people" and "profile" into "profiles".

4). We are also using the [jade](http://jade-lang.com/) template engine to create an HTML template of our `<select>` element. The reason we didn't simply write HTML was to take advantage of jade's string interpolation rather than having to concatenate a bunch of strings with plus signs everywhere.

This

	"select(ng-model='#{attribute}',"ng-options=...

Instead of

  {% highlight html %}
  	"<select ng-model=" + attribute + " " + "ng-options=" + .... + .... "</select>"
  {% endhighlight %}

This is simply a matter of preference, but I feel it provides better readability.

5). We are then using Angular's `$compile` service which takes in an HTML string and returns a template function, allowing us to then link the scope and the template together.

6). Lastly, we are injecting the HTML `<select>` elements into the DOM.

We are now able to use our custom directives in our HTML to encapsulate a lot of this logic and reduce duplication.

Here's a working example:

<iframe width="100%" height="300" src="http://jsfiddle.net/danny__garcia/me6horzf/3/embedded/result,html,js" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

## Conclusion

Creating this custom directive wasn't necessary to solve the problem of filtering through profiles based on their status, but the purpose of this project is to learn new things. Understanding how to build complex custom directives can allow you to build powerful AngularJS applications. There are many different ways to create custom template-expanding directives and this is just my first stab at it. Have you had experience in the past building custom directives? If you know of other/better ways to solve this problem with custom directives please leave your comments below or fork the JSFiddle above and let me know what you think.