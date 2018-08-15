---
title: "Dnn Localized Logo"
date: 2018-05-03T21:25:33-05:00
draft: false
description: ""
tags: ["DNN","Tutorial"]
---

Often times I find myself hard coding some parts of site directly into the skin files simply because it's something that will rarely if every change for the client e.g. a phone number, address, logo, etc. I need for it to be within certain confines of the design and it's not worth trying to implement into some module so in the skin it goes.

Recently though, I had a project that needed to be localized for three different countries. Now all of these were going to be in English BUT guess what needed to be different for all of them? You guessed it, the logo and phone number.

This is pretty easy to do. First we just set a variable with the value of the current language. This is done with DNN's `PageCulture.Name`. I go the extra step to make it lower case and trim any whitespace just to be sure but this isn't necessary.

{{< highlight asp >}}
<%
    string language = (Page as DotNetNuke.Framework.PageBase).PageCulture.Name.ToLower().Trim();
%>
{{< /highlight >}}

Then where ever you need to switch layout / content based on the country, you just write a conditional against the previously defined variable for the current country.

{{< highlight asp >}}
<% if (language == "en-ca") { %>
    <!-- Your code here -->
<% } else { %>
    <!-- Your code here -->
<% } %>
{{< /highlight >}}

### Why not use the logo skin object? ###
Now I can already here some of you saying, "Just use the core logo skin object". And yes this would work. It would allow me to change the logo through DNN's interface for the different countries but in my case I wanted to use an inline SVG for the logo. This reduces http requests and it allows more control with css. You can read [all about inline SVGs here](https://css-tricks.com/using-svg/). So the logo object wouldn't work for this need.

### The other thing ###
There is one good counter argument and that is to use DNN's core text skin object. For most of my cases, even when dealing with inline svg logos, this would do quite nicely, but if for whatever reason you needed to alter the layout even just a little, then you would need some logic and the example would do just the trick. If you want to read more about the text skin object, here's a great article on [using the DNN text skin object](http://www.dnnsoftware.com/community-blog/cid/132018/how-to-use-the-text-dotnetnuke-skin-object) by [Will Strohl](https://twitter.com/WillStrohl).