# Using ngMessages / ngMessage

## Overview

When our inputs have lots of validation rules, writing out conditional `ng-if` statements for *every* type of validation can be very verbose. This is where a new module comes in - `ngMessages`!

## Objectives

- Describe ngMessages/ngMessage
- Use ngMessages to display different types of validation errors

## ngMessages

`ngMessages` is a separate module for Angular, so we need to make sure we require the file and module in our main module.

```js
angular
	.module('app', [
		'ngMessages'
	]);
```

And then we're ready to use it!

## What does it do?

`ngMessages` allows us to take an object and display different messages depending on what properties exist on that object.

This links in with our `$error` object that we had in the previous README. To refresh your memory, the `$error` object may look something like this for a field named `username`:

```js
{
	username: {
		$error: {
			required: true
		}
	}
}
```

Let's pass the `$error` object into `ngMessages`. As a parent component, we use the `ng-messages` directive.

```html
<form name="form">
	<input
	    name="username"
		ng-model="ctrl.username"
		required="required" />

	<div ng-messages="form.username.$error">
	</div>
</form>
```

Now that we've done that, we can use the directive `ng-message` for each property for which we want to display errors. As we've only got the possibility of `required` appearing in our object (as that is the only validation we've defined), we'll have one element:

```html
<form name="form">
	<input
	    name="username"
		ng-model="ctrl.username"
		required="required" />

	<div ng-messages="form.username.$error">
		<div ng-message="required">Username is required!</div>
	</div>
</form>
```

We can use this on different inputs too. For instance, with an email, we get an `email` property. We can also add a `minlength` validation:

```html
<form name="form">
	<input
	    name="email"
	    type="email"
		ng-model="ctrl.email"
		minlength="2"
		required="required" />

	<div ng-messages="form.email.$error">
	</div>
</form>
```

We'll now have an object that looks like this:

```js
{
	email: {
		$error: {
			required: true,
			minlength: true,
			email: true
		}
	}
}
```

And now we can simply add more DOM elements for each error message:

```html
<form name="form">
	<input
	    name="email"
	    type="email"
		ng-model="ctrl.email"
		minlength="2"
		required="required" />

	<div ng-messages="form.email.$error">
		<div ng-message="required">Email is required!</div>
		<div ng-message="minlength">Must be more than 2 characters!</div>
		<div ng-message="email">Must be a valid email!</div>
	</div>
</form>
```

It's worth noting that only one message will be displayed at a time, in order of the DOM nodes. At first, our `required` message will show, then if we type one letter in our input, the `minlength` message will show. Then when we've typed more letters, the `email` message will appear.

## Only on $touched

We can also then add an `ng-if` to only show the error messages once the user has actually interacted with the input.

```html
<form name="form">
	<input
	    name="email"
	    type="email"
		ng-model="ctrl.email"
		minlength="2"
		required="required" />

	<div ng-messages="form.email.$error" ng-if="form.email.$touched">
		<div ng-message="required">Email is required!</div>
		<div ng-message="minlength">Must be more than 2 characters!</div>
		<div ng-message="email">Must be a valid email!</div>
	</div>
</form>
```

Awesome! Let's compare this to what our previous code would've been:

```html
<div ng-if="form.email.$touched">
    <div ng-if="form.email.$error.required">
        Email is required!
    </div>
    <div ng-if="form.email.$error.minlength">
        Email must be more than 2 characters!
    </div>
    <div ng-if="form.email.$error.email">
        Must be a valid email!
    </div>
</div>
```

You'll see we have cut down a lot of repeated code - also if we were to change the name of our input, we'd only have to change it once with `ngMessages` - 3 times (and maybe many more) without it.

<p class='util--hide'>View <a href='https://learn.co/lessons/angular-using-ngmessages-readme'>Using Ngmessages </a> on Learn.co and start learning to code for free.</p>
