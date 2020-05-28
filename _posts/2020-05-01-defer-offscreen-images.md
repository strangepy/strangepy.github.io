---
title: Exploring How to Defer Offscreen Images 
subtitle: using only JavaScript
image: /img/jon-tyson-FlHdnPO6dlw-unsplash.jpg
tags: [web-performance, javascript]
---

[Google PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/) is a wonderful tool for improving your website. However, it can also be difficult to tell which improvements you should focus on first. For mobile devices, deferring offscreen images can improve page speed significantly. But keeping track of the user's scroll position can be costly, and you do not want to trade download time for processing time. This post will explore an option using pure JavaScript that does not rely on scroll position. 

## The Solution 

First off, how do you even know if this post applies to you?? You can use an automated reporting tool, such as Lighthouse or PageSpeed Insights, to check for offscree images that should be deferred. In most browsers, you can open the developer console, click on the Audits or Lighthouse tab, and then click Run Report to kick off a new report. Otherwise, you can visit the [PageSpeed Insights website](https://developers.google.com/speed/pagespeed/insights/) and paste in the URL you want to audit. If "Defer offscreen images" is in the list of improvements, then this post will apply to you! Here is an example of what that recommendation looks like in my browser. 

![Deferring offscreen images on this page is recommended](/img/lighthouse_defer_offscreen_images_before.png)

Now that you know this post applies to you, we can cover the general approach. The basic idea of this approach is to load in a small base-64 image as a placeholder until the onload event, and then after the onload event we will replace the placeholder with the actual image. This process is based largely on the [wonderful post here.](https://varvy.com/pagespeed/defer-images.html)

## Encoding an Image

For this website, we're going to use the strangePy logo as the placeholder image. All you need to do is upload your desired image to an encoder website, such as [base64-image.de](https://www.base64-image.de/) and then copy the output. Below is the raw output of the base64 encoded image. 

```
data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAJAAAACQCAQAAABNTyozAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAAmJLR0QA/4ePzL8AAAAHdElNRQfkAQECMiOITkyMAAADPklEQVR42u3dTUiTcQDH8d9yYQeNioKKoDdJKjoICWGX6jiCTkJh5MFLnTpYBwnxJNQtCuwQVIQQeQkhIgkKqUMlHQSJQupSRNgbFmjZix3G3Nvz7Den25j7fnbZ/v//nmfPl82NqZsEAAAAAEC4Uc0W4TSpPbl2uqzcR112KzWkzeHTlRRotkjb3aB7WhU2SSBJ2qU7qg2eIlDcAV1XJGiCQAnH1B00HC33Uc/DwgJ91zU90ZQa1aa9gSt69EoD5T7IhRhZwJP5U62f205EZ0JWTWt/5k4j87qJqa6qqYgxOjWcNfZczQVu7bN2ayJtpE+nQla2aHxxDmG4KC/bEqcjAXt8VvDWzmVta51mQtaOa23qwur4IT2UNfJJL0LWNmhQK5IXqyPQt4Cxr6GrW3Qz2aU6Am0LGNueY32rzifOVkegtqyRZjXmvMZZnYyfqaRAhTuug2mXa9Vnr3NZMamyAhV+D6rRoNrnjnWHHoS8VEwV1W01Vc8r6XrdUI8e6q+26FCeR12nu9pXOYE2pbwWLsxWdczzGht1q3IeYrvVUPJ9/lJX5QQqvVmd0GMChevWQGU9i5XWFfVKBAozpNPxMwQKMqpW/Y6fJVC2d4rpR+ICgTJNKqYPyYsESvdHRzWWOlDsQGsUyTj1lrtBTp26nz7APSjVBV3KHCJQUr+6sgcJlPBIHUHvFxAo7rVaNRM0QSBJmlBMX4KnCCRN6bDehk0S6J/aNRI+XTnvKOanXxfneY1pvcw1vdQCfQz9jWmBeIgZBDIIZBDIIJBBIINABoGMaOAfF2Wa0fty39ByiepNHqvGcv9HzFLGQ8wgkEEgg0AGgQwCGQQyCGQQyCCQQSCDQAaBDAIZBDIIZBDIIJBBIINABoEMAhkEMghkEMggkEEgg0AGgQwCGQQyCGQQyCCQQSCDQAaBDAIZBDIIZBDIIJBBIINABoEMAhkEMghkEMggkEEgg0AGgQwCGfl9wNLOgO8xqS/q7epPfBrvnOWlSpIuv0A1Wl3i21VX+hTBeIgZBDIIZBDIIJBBIINABoEMAhkEMghkEMggkEEgg0AGgYxo4PcgL57sr/L4WdQ9Thf1aAAAAAAAJfEfaGgQuLoUUnMAAAAldEVYdGRhdGU6Y3JlYXRlADIwMjAtMDEtMDFUMDI6NTA6MzUrMDE6MDAAFiETAAAAJXRFWHRkYXRlOm1vZGlmeQAyMDIwLTAxLTAxVDAyOjUwOjM1KzAxOjAwcUuZrwAAAFd6VFh0UmF3IHByb2ZpbGUgdHlwZSBpcHRjAAB4nOPyDAhxVigoyk/LzEnlUgADIwsuYwsTIxNLkxQDEyBEgDTDZAMjs1Qgy9jUyMTMxBzEB8uASKBKLgDqFxF08kI1lQAAAABJRU5ErkJggg==
```

The text above looks lengthy, but we're not going to be typing that in anywhere. This is what the image looks like when it actually renders on the page (you can even inspect it in your browser to see how it works).

<img id='base64image' src='data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAJAAAACQCAQAAABNTyozAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAAmJLR0QA/4ePzL8AAAAHdElNRQfkAQECMiOITkyMAAADPklEQVR42u3dTUiTcQDH8d9yYQeNioKKoDdJKjoICWGX6jiCTkJh5MFLnTpYBwnxJNQtCuwQVIQQeQkhIgkKqUMlHQSJQupSRNgbFmjZix3G3Nvz7Den25j7fnbZ/v//nmfPl82NqZsEAAAAAEC4Uc0W4TSpPbl2uqzcR112KzWkzeHTlRRotkjb3aB7WhU2SSBJ2qU7qg2eIlDcAV1XJGiCQAnH1B00HC33Uc/DwgJ91zU90ZQa1aa9gSt69EoD5T7IhRhZwJP5U62f205EZ0JWTWt/5k4j87qJqa6qqYgxOjWcNfZczQVu7bN2ayJtpE+nQla2aHxxDmG4KC/bEqcjAXt8VvDWzmVta51mQtaOa23qwur4IT2UNfJJL0LWNmhQK5IXqyPQt4Cxr6GrW3Qz2aU6Am0LGNueY32rzifOVkegtqyRZjXmvMZZnYyfqaRAhTuug2mXa9Vnr3NZMamyAhV+D6rRoNrnjnWHHoS8VEwV1W01Vc8r6XrdUI8e6q+26FCeR12nu9pXOYE2pbwWLsxWdczzGht1q3IeYrvVUPJ9/lJX5QQqvVmd0GMChevWQGU9i5XWFfVKBAozpNPxMwQKMqpW/Y6fJVC2d4rpR+ICgTJNKqYPyYsESvdHRzWWOlDsQGsUyTj1lrtBTp26nz7APSjVBV3KHCJQUr+6sgcJlPBIHUHvFxAo7rVaNRM0QSBJmlBMX4KnCCRN6bDehk0S6J/aNRI+XTnvKOanXxfneY1pvcw1vdQCfQz9jWmBeIgZBDIIZBDIIJBBIINABoGMaOAfF2Wa0fty39ByiepNHqvGcv9HzFLGQ8wgkEEgg0AGgQwCGQQyCGQQyCCQQSCDQAaBDAIZBDIIZBDIIJBBIINABoEMAhkEMghkEMggkEEgg0AGgQwCGQQyCGQQyCCQQSCDQAaBDAIZBDIIZBDIIJBBIINABoEMAhkEMghkEMggkEEgg0AGgQwCGfl9wNLOgO8xqS/q7epPfBrvnOWlSpIuv0A1Wl3i21VX+hTBeIgZBDIIZBDIIJBBIINABoEMAhkEMghkEMggkEEgg0AGgYxo4PcgL57sr/L4WdQ9Thf1aAAAAAAAJfEfaGgQuLoUUnMAAAAldEVYdGRhdGU6Y3JlYXRlADIwMjAtMDEtMDFUMDI6NTA6MzUrMDE6MDAAFiETAAAAJXRFWHRkYXRlOm1vZGlmeQAyMDIwLTAxLTAxVDAyOjUwOjM1KzAxOjAwcUuZrwAAAFd6VFh0UmF3IHByb2ZpbGUgdHlwZSBpcHRjAAB4nOPyDAhxVigoyk/LzEnlUgADIwsuYwsTIxNLkxQDEyBEgDTDZAMjs1Qgy9jUyMTMxBzEB8uASKBKLgDqFxF08kI1lQAAAABJRU5ErkJggg=='/>

Now that we have the base-64 encoded image ready to go, we can start implementing the image deferral logic into the website. First, we'll want to write a few lines of Javascript to swap the encoded placeholder image for the proper image. Then, we'll want to update our webpages to use the encoded placeholder image by default, and add a `data-src` parameter onto the `<img>` tag in order to hold the proper image source. 

## JavaScript 

To get things started, we want to write a function to swap out the image source of one image. We should check to make sure the `data-src` parameter is attached to the image. If it is, then we can set the `src` attribute from the base-64 encoded placeholder image to the `data-src` attribute containing the path to the proper image. 

```JavaScript 
function deferImg(image){
    if (image.getAttribute('data-src')) {
            image.setAttribute('src', image.getAttribute('data-src'));
        }
}
```

Next, we'll grab all of the images and put them into one array so we can later apply the function to each image in this array. 

```JavaScript 
var images = document.querySelectorAll('img');
```

We have all the logic set up, but nothing to actually run the new function. This implementation may vary depending on if you are using jQuery or another JavaScript library. For this example, we'll use pure JavaScript and an event listener for the `document.state` to be `complete`. 

```JavaScript
document.addEventListener('readystatechange', () => {    
  if (document.readyState == 'complete') images.forEach(deferImg);
});
```

That's it! Just a few lines of JavaScript and we are all set to add the encoded placeholder images. For completeness, here is all of the JavaScript logic put together. 

```JavaScript 
var images = document.querySelectorAll('img');

function deferImg(image){
    if (image.getAttribute('data-src')) {
            image.setAttribute('src', image.getAttribute('data-src'));
        }
}

document.addEventListener('readystatechange', () => {    
  if (document.readyState == 'complete') images.forEach(deferImg);
});
```

## HTML 
The specific HTML will be a bit different depending on how your site is structured. In general, you should have an image with a `src` attribute by default, and you want to switch the `src` value into the `data-src` parameter, and then set the `src` to your base-64 encoded placeholder image. 
```HTML 
<!-- Before -->
<img src="/img/mstile-144x144.png" >

<!-- After -->
<img data-src="/img/mstile-144x144.png" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAJAAAACQCAQAAABNTyozAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAAmJLR0QA/4ePzL8AAAAHdElNRQfkAQECMiOITkyMAAADPklEQVR42u3dTUiTcQDH8d9yYQeNioKKoDdJKjoICWGX6jiCTkJh5MFLnTpYBwnxJNQtCuwQVIQQeQkhIgkKqUMlHQSJQupSRNgbFmjZix3G3Nvz7Den25j7fnbZ/v//nmfPl82NqZsEAAAAAEC4Uc0W4TSpPbl2uqzcR112KzWkzeHTlRRotkjb3aB7WhU2SSBJ2qU7qg2eIlDcAV1XJGiCQAnH1B00HC33Uc/DwgJ91zU90ZQa1aa9gSt69EoD5T7IhRhZwJP5U62f205EZ0JWTWt/5k4j87qJqa6qqYgxOjWcNfZczQVu7bN2ayJtpE+nQla2aHxxDmG4KC/bEqcjAXt8VvDWzmVta51mQtaOa23qwur4IT2UNfJJL0LWNmhQK5IXqyPQt4Cxr6GrW3Qz2aU6Am0LGNueY32rzifOVkegtqyRZjXmvMZZnYyfqaRAhTuug2mXa9Vnr3NZMamyAhV+D6rRoNrnjnWHHoS8VEwV1W01Vc8r6XrdUI8e6q+26FCeR12nu9pXOYE2pbwWLsxWdczzGht1q3IeYrvVUPJ9/lJX5QQqvVmd0GMChevWQGU9i5XWFfVKBAozpNPxMwQKMqpW/Y6fJVC2d4rpR+ICgTJNKqYPyYsESvdHRzWWOlDsQGsUyTj1lrtBTp26nz7APSjVBV3KHCJQUr+6sgcJlPBIHUHvFxAo7rVaNRM0QSBJmlBMX4KnCCRN6bDehk0S6J/aNRI+XTnvKOanXxfneY1pvcw1vdQCfQz9jWmBeIgZBDIIZBDIIJBBIINABoGMaOAfF2Wa0fty39ByiepNHqvGcv9HzFLGQ8wgkEEgg0AGgQwCGQQyCGQQyCCQQSCDQAaBDAIZBDIIZBDIIJBBIINABoEMAhkEMghkEMggkEEgg0AGgQwCGQQyCGQQyCCQQSCDQAaBDAIZBDIIZBDIIJBBIINABoEMAhkEMghkEMggkEEgg0AGgQwCGfl9wNLOgO8xqS/q7epPfBrvnOWlSpIuv0A1Wl3i21VX+hTBeIgZBDIIZBDIIJBBIINABoEMAhkEMghkEMggkEEgg0AGgYxo4PcgL57sr/L4WdQ9Thf1aAAAAAAAJfEfaGgQuLoUUnMAAAAldEVYdGRhdGU6Y3JlYXRlADIwMjAtMDEtMDFUMDI6NTA6MzUrMDE6MDAAFiETAAAAJXRFWHRkYXRlOm1vZGlmeQAyMDIwLTAxLTAxVDAyOjUwOjM1KzAxOjAwcUuZrwAAAFd6VFh0UmF3IHByb2ZpbGUgdHlwZSBpcHRjAAB4nOPyDAhxVigoyk/LzEnlUgADIwsuYwsTIxNLkxQDEyBEgDTDZAMjs1Qgy9jUyMTMxBzEB8uASKBKLgDqFxF08kI1lQAAAABJRU5ErkJggg==">
```

The above is an example for one specific image. You likely don't want to manually do this for every single one of your images on your website. How to set it for your website may be different than mine.  My website is hosted on GitHub pages and uses Jekyll with Liquid variables, so the change is quite a bit more concise here. 

```HTML
<!-- Before -->
<img src="{{ post.image | relative_url }}" >
     

<!-- After -->
<img src="{{ site.encodedlogo }}" data-src="{{ post.image | relative_url }}">
```

Note that the above is only possible due to the support for Liquid variables. If you do not use the Liquid templating engine in your website, then this approach will not work, but you should have something similar for dynamically loading in images without manually setting each one. One more note on the example above, I saved the base-64 encoded image into a site variable named `encodedlogo` so there is just one easy place for me to update the encoded image in the future if I ever need to. 

## Closing Thoughts
This should be all you need to do on your website! Just encode an image (such as your logo), enter the encoded image into your `src` attribute, and set the `data-src` attribute to the proper image reference. The few lines of JavaScript that we wrote earlier will take care of the rest! 

For a quick sanity check, we can run another audit and confirm that "Defer offscreen images" is no longer one of the recommendations. You can expand the "Passed audits" section to verify that the check has passed. 

![Offscreen images have been successfully deferred](/img/lighthouse_defer_offscreen_images_after.png)

## Further Reading
- [The original article written by Patrick Sexton that inspired this post.](https://varvy.com/pagespeed/defer-images.html)
