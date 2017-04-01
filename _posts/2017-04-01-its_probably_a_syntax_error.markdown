---
layout: post
title:  "It's probably a syntax error"
date:   2017-04-01 23:43:05 +0000
---


I was working on the ZHW Shoes Layout lab, tearing my hair out, and wondering why my columns were still at 100% width, even though I was sure that I had made the right changes in the CSS. I looked and looked, and just couldn't figure out what I had done wrong. Eventually, I began wondering if I had understood the concept at all, and was starting to question my life choices...

After far too long, I realized that I had included two class attributes in an element:

![Screenshot of syntax error](http://i.imgur.com/ngNvGOc.jpg)

That'll teach me for completing labs right after waking up. The lab took me much longer than it would have, had I just run my code through a CSS and HTML validator. Here's what I would have seen right away, had I thrown my code into the [W3C HTML Validator](https://validator.w3.org/#validate_by_input):

![Screenshot of results of W3C HTML validator](http://i.imgur.com/sgehjsQ.jpg)

When inspecting the problem element in Chrome Dev Tools, I could see that the browser had ignored the second class attribute altogether, so no wonder I wasn't seeing any of the styles that I thought I had applied: 

![Screenshot of the element in Dev Tools](http://i.imgur.com/144k2UL.jpg)

I felt really silly, partly because of the error, but mostly because I had forgotten to use all of the tools at my disposal to help me with debugging.

This was a simple error, and I know that it can (and will!) be much more complicated in the future, so this was a good reminder to take a deep breath, slow down, and follow clear steps to resolve an issue. I still have some work to do in learning what all those steps are, so it's time to stop procrastinating and learn how to use Chrome Dev Tools already.



