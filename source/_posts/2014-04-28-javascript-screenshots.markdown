---
layout: post
title: "JavaScript Screenshots"
subtitle: "User reconnaissance with HTML2Canvas"
date: 2014-04-28 00:39:54 -0400
comments: true
categories: 
---

One of my current projects — [GitShoes](http://www.gitshoes.com/ — takes on the important task of user feedback. I was able to build out the functionality of creating embeddable JavaScript feedback widgets pretty quickly. And getting them to submit issues to the correct GitHub repository through [Octokit](https://github.com/octokit/octokit.rb) was pretty straightforward. However, we quickly realized that users didn’t usually provide enough feedback for a developer to easily solve the problem. A few words (especially from a non-technical user) might not lead the developer to the root of the problem, so we decided to take things one step further.

> “A picture is worth a thousand words.”

Our average issue was about 20-30 words long. So, if a picture is worth 1000 words, that means taking some quick screenshots at issue submission would make our app about 40x more useful. Seems like a no-brainer feature to me. But how do we make it happen?

### Screenshots aren’t built into JavaScript

Well, that would have been convenient. Fortunately, HTML5 introduced the Canvas element. Here’s W3C’s explanation of the [Canvas](http://www.w3schools.com/html/html5_canvas.asp) element:

> The HTML5 <canvas\> element is used to draw graphics, on the fly, via scripting (usually JavaScript).
> The <canvas\> element is only a container for graphics. You must use a script to actually draw the graphics.

The canvas sounds like the perfect place for our screenshot to live, but how can we draw on it? I knew I couldn’t be the first person with a need for this, so I did what any reasonable developer would do: look to Open Source for a solution.

### Enter HTML2Canvas

[HTML2Canvas](https://github.com/niklasvh/html2canvas) is an amazing JavaScript library by [Niklas von Hertzen](https://github.com/niklasvh). Here’s what you need to know about it:

* **HTML2Canvas is entirely client-side.** Everything it does happens within your user’s browser, not on your server. This fact comes with its pros and cons. Pro: Repeatedly performing these operations on your own server would be very costly, but you don’t have to. Con: Your user’s browser environment is less reliable than your server’s. There’s a greater probability of something going wrong. You also need to send the image back in the form of a data url, which assumes a fairly powerful connection for the client.
* **HTML2Canvas is recreating elements** from the page (or the whole page) from the DOM. You must have a canvas available on the page in which to store the elements you recreate.
* **HTML2Canvas only writes to the canvas.** If you need to send your image somewhere, like your server, it’s up to you to handle that.

### Writing to the canvas

The first step to consider is when you want your screenshot to be taken, likely either when the DOM is fully loaded or when something is clicked. For me, this was on clicking the widget, indicating you want to provide feedback. You’ll want to work within an event listener for that action.

```javascript
$('div#gitshoes-button').on('click', function() {
    // Your code goes here.
}
```

Next, you must determine what you would like to take a screenshot of — the entire document or a select element. I chose to use the entire document.

```javascript
$('div#gitshoes-button').on('click', function() {
    html2canvas(document.body, {
        // Your code goes here.
    });
}
```

Now, all that’s left is to tell HTML2Canvas where to place our newly canvased screenshot.

```javascript
$('div#gitshoes-button').on('click', function() {
    html2canvas(document.body, {
        onrendered: function(canvas) {
            $('#gitshoesCanvas').remove();
            $('body').append(canvas).find('canvas').last() \
            .css('display', 'none') \
            .attr('id', 'gitshoesCanvas');
            formDiv.slideDown(); /* Ignore this line */
        }
    });
}
```

In the above code, I am doing a few things. First, I am removing any canvas I may have generated earlier in the pageview. Otherwise, each consecutive screenshot will take twice as long as the previous because it is recreating the existing canvas, even though it is hidden. Second, I am appending the newly created canvas to the body and applying CSS styles to hide it (so users don’t see a duplicate of their screen below) and make it easily findable. Lastly, I am revealing the feedback widget, but that isn’t relevant to the rest of the canvasing function. If you don’t need to hide your canvas or remove the existing canvas, you could get by without lines 4, 6, 7, 8 and the second half of line 5.

### Converting the canvas to a data url

HTML is the language of the browser. If you plan on sending the screenshot back to your server or to a third-party, like AWS, it should be converted to something more manageable, like a data url (base64 encoded data). This is as simple as one line of code.

```javascript
var image_data_url = $('canvas').last()[0].toDataURL();
```

### What’s next?

The data url we created from the canvas is ready to be sent back to the server. Once you have the image on your server, there are countless things you can do with it. I chose to store it in a tempfile and upload it to AWS using [Fog](https://github.com/fog/fog), which is a great, lightweight cloud services gem, but that’s beyond the scope of this blog post. After being uploaded to AWS, the screenshots generated from the code above look like [this](https://s3-us-west-2.amazonaws.com/gitshoes/screenshot20140423-49700-5aiovm.png).

### Go forth and take some screenshots!
For such a powerful tool, HTML2Canvas is relatively easy to implement.

* Check out the [documentation](https://github.com/niklasvh/html2canvas)
* Find a way to add value to one of your projects through screenshots
* Be responsible — don’t expose sensitive information from your users