---
layout: post
title: "Styling a horizontal rule"
date: 2013-07-22 10:54
comments: true
categories: 
- html
- css
---
Up until recently I didn't realize you could style horizontal rules (`<hr />`). Of course you could use a div to do the same thing, but using a horizontal rule for its intended use is nice (and easier than using a div).

Here's the CSS I used most recently on a website I'm making to customize the horizontal rule.

	hr {
		width: 90%;
		border: 0;
		height: 1px;
		background: #000;
		opacity: 0.2;
	}

Of course you can also name the style and apply it to the horizontal rule.

	// CSS
	hr.custom {
		width: 90%;
		border: 0;
		height: 1px;
		background: #000;
		opacity: 0.2;
	}

	// HTML
	<hr class="custom" />

You can also do things like gradients, blurs, etc.