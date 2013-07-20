---
layout: post
title: "Rounded corner text inputs"
date: 2013-07-20 12:02
comments: true
categories:
- html
- css
---
The easiest way to create a custom text input on a form is to remove the default border of the input and place a custom image behind it as its background.

Here's the background I'm using:

<p align="center">
  <img src="images/search_bg.png" />
</p>

Here's the HTML.

    <div id="search">
      <form method="POST" action="#" accept-charset="UTF-8">
        <input class="search" type="text">  	
      </form>
    </div>

And the CSS.

    #search {
	    width: 362px;
	    height: 41px;
	    background: url('/images/search_bg.png') no-repeat left top;
	    line-height: 41px;
	    margin: 10px auto 10px auto;
	    text-align: center;
    }

	.search {
		border: none;
		text-align: center;
		outline-width: 0;
	}

The `line-height` property sets the line-height to the same height as the div height. This has the effect of vertically centering the input (and hence the text) in the div.

The `border` property is the one that makes the default text input's background invisible, and then we set the background of the containing div to the image we created with the `background` property.

The `margin` property is used to add a 10px margin to the top and bottom of the div. The `auto` attributes are used to center the div (and hence the input) in the middle of the page.
