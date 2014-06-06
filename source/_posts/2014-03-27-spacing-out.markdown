---
layout: post
title: "Spacing Out"
subtitle: "How to prevent gross typos once and for all"
date: 2014-03-27 00:18:52 -0400
comments: true
categories: 
---

A couple weeks ago, I was scraping Wikipedia and hit a wall while I was parsing the data. I eventually traced my errors back to my parser, which was failing to split a string on a space I knew should be present in each iteration. I dug further, and eventually isolated the iteration causing the failure. I copy and pasted the string into IRB to test the split myself and, as expected, the string couldn’t be split on the space. *But the space did exist*. It was there. I could see it. I tried writing the same string out by hand and splitting it. That worked.
<!--more-->

![My brain is full of fuck](https://d262ilb51hltx0.cloudfront.net/max/1000/1*O7CIpHjHmv7AvIY6n1izdA.png)

Did that not make any sense? Download this [gist](https://gist.github.com/pat-whitrock/9776536#file-encodings) and try running the commands in IRB, PRY. They’re identical commands, but with different return values. If we’re splitting the same string on a space and getting different return values, it can only mean one thing — one of the spaces isn’t a space. But it’s clearly a space… I can see it. It’s a space. So, the real question is this…

### When is a space not a space?

A space is a space is a space, right? Wrong. There are lots of types of spaces. And when my representation of a space is different from your representation of a space, the world explodes (or we can’t split strings). The reason I couldn’t split my string is the same fundamental reason you’re periodically annoyed by weird characters like �.

Different coded character sets are encoded differently. Computers outdate the internet, so character encoding wasn’t designed with interlingual string transfer in mind. Instead, we developed encoding standards optimized for our cultural contexts. In the realm of character encoding, these cultural contexts are referred to as **character sets**. For example, the the standard English characters we are accustomed to are of a different character set than that of the Spanish language or Chinese.

We developed different **coded character sets** for these different cultural contexts without the foresight that we might one day want to exchange information with each other through strings on such a massive scale. A coded character set is basically the combination of character codes and their respective characters.

![ASCII / American Standard Code for Information Interchange](https://d262ilb51hltx0.cloudfront.net/max/600/1*djp9tqvUr9wA5lPw31lALQ.gif)

ASCII became the American standard for encoding the characters we used. It could store all the needed characters in 7 bits, leaving a convenient 8th bit of **character encodings** for you to do weird stuff with, like make your computer beep at innocent bypassers.

Everyone else developed their own encoding standards for their language’s character set. There were even multiple coded character sets for the same real-life character set. Eventually, the west agreed on the ANSI standard, so we were clear on what the first 128 of 256 8-bit characters would represent, but left the remaining 128 for customization. However, Asian languages with exponentially more characters were never going to be able to fit their character sets within 8 bits, so they went down a completely different path.

### One encoding standard to rule them all

Enter Unicode. The Unicode consortium sought to create a single character set including all of the world’s writing systems (and Klingon). The Unicode standard consists of a set of code charts for visual reference, an encoding method, set of standard [character encodings](http://en.wikipedia.org/wiki/Character_encoding) and a set of reference data [computer files](http://en.wikipedia.org/wiki/Computer_file).

Unicode gave birth to the most commonly used character encodings in use today, most notably **UTF-8**. UTF-8 encodes ASCII characters in one byte, with the same codes used by ASCII. Its remaining characters are encoded using up to 4 bytes.

### Back to the spaces

After doing some research, I stumbled upon a super helpful method for inspecting seemingly identical strings: **codepoints**.

```ruby
"Hello world".codepoints
	=> [72, 101, 108, 108, 111, 32, 119, 111, 114, 108, 100]
"Hello world".codepoints
	=> [72, 101, 108, 108, 111, 160, 119, 111, 114, 108, 100]
```

Notice the difference? When you split a string on a space, you’re splitting on the integer ordinal of 32. For some strange reason, Wikipedia formatted one of 3,000 datapoints with a unicode non-breaking space, also known as integer ordinal 160. Why? So I could share this valuable learning experience with you, of course!

### Takeaways

1. Sometimes characters act differently than you would expect. In actuality, they might not be the same characters you think they are. Impostors!!!
2. Characters can’t lie to codepoints. Make them show their true selves if they’re acting shady.
3. If it looks like a space, but doesn’t act like a space, try this:

```ruby
"Hello world".split(/[[:space:]]/)
	=> ["Hello", "world"]
```

![Knowledge!](https://d262ilb51hltx0.cloudfront.net/max/800/1*2JDxyN3qdMoMx8WYDafyaQ.png)