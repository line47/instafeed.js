instafeed.js
============
Instafeed is a dead-simple way to add Instagram photos to your website. No jQuery required, just good 'ol plain javascript.

## Installation
Setting up Instafeed is pretty straight-forward. Just download the script and include it in your HTML:

```html
<script type="text/javascript" src="path/to/instafeed.min.js"></script>
```

Instafeed.js also supports AMD/CommonJS

```js
// AMD
require(["path/to/instafeed"], function(Instafeed) {

});

// CommonJS
var Instafeed = require("instafeed");
```

### NPM/Bower

Instafeed.js is also available on NPM and Bower:

```sh
npm install instafeed.js      # npm
bower install instafeed.js    # bower
```

## Basic Usage

Here's how easy it is to get all images tagged with __#awesome__:

```html
<script type="text/javascript">
    var feed = new Instafeed({
        feedId: 'YOUR_BITSALAD_FEED_ID'
    });
    feed.run();
</script>
```

Instafeed with automatically look for a `<div id="instafeed"></div>` and fill it with linked thumbnails. Of course, you can easily change this behavior using [standard options](#standard-options). Also check out the [advanced options](#advanced-options) for some advanced ways of customizing __Instafeed.js__.

## Requirements

The only thing you'll need to get going is a __feed id__ from [BitSalad.co](http://bitsalad.co) - visit there now to easily register your Instagram account.

- `feedId` - __Required__. Your feed ID from bitsalad.co
- `target` - Either the ID name or the DOM element itself where you want to add the images to.
- `template` - Custom HTML template to use for images. See [templating](#templating).
- `sortBy` (string) - Sort the images in a set order. Available options are:
    - `none` (default) - As they come from Instagram.
    - `most-recent` - Newest to oldest.
    - `least-recent` - Oldest to newest.
    - `most-liked` - Highest # of likes to lowest.
    - `least-liked` - Lowest # likes to highest.
    - `most-commented` - Highest # of comments to lowest.
    - `least-commented` - Lowest # of comments to highest.
    - `random` - Random order.
- `links` - Wrap the images with a link to the photo on Instagram.
- `limit` - Maximum number of Images to add. __Max of 60__.
- `useHttp` - By default, image urls are protocol-relative. Set to `true`
  to use the standard `http://`.
- `resolution` - Size of the images to get. Available options are:
    - `thumbnail` (default) - 150x150
    - `low_resolution` - 306x306
    - `standard_resolution` - 612x612

## Advanced Options

- `before` (function) - A callback function called before fetching images from Instagram.
- `after` (function) - A callback function called when images have been added to the page.
- `success` (function) - A callback function called when Instagram returns valid data. (argument -> json object)
- `error` (function) - A callback function called when there is an error fetching images. (argument -> string message)
- `mock` (bool) - Set to true fetch data without inserting images into DOM. Use with __success__ callback.
- `filter` (function) - A function used to exclude images from your results. The function will be
  given the image data as an argument, and expects the function to return a boolean. See the example
  below for more information.


To see a full list of properties that `image` has, see [issue #21](https://github.com/stevenschobert/instafeed.js/issues/21).

## Templating

The easiest way to control the way Instafeed.js looks on your website is to use the __template__ option. You can write your own HTML markup and it will be used for every image that Instafeed.js fetches.

Here's a quick example:

```html
<script type="text/javascript">
    var feed = new Instafeed({
        feedId: 'YOUR_BITSALAD_FEED_ID',
        template: '<a class="animation" href="{{link}}"><img src="{{image}}" /></a>'
    });
    feed.run();
</script>
```

Notice the `{{link}}` and `{{image}}`? The templating option provides several tags for you to use to control where variables are inserted into your HTML markup. Available keywors are:


- `{{type}}` - the image's type. Can be `image` or `video`.
- `{{width}}` - contains the image's width, in pixels.
- `{{height}}` - contains the image's height, in pixels.
- `{{orientation}}` - contains the image's orientation. Can be `square`, `portrait`, or `landscape`.
- `{{link}}` - URL to view the image on Instagram's website.
- `{{image}}` - URL of the image source. The size is inherited from the `resolution` option.
- `{{id}}` - Unique ID of the image. Useful if you want to use [iPhone hooks](http://instagram.com/developer/iphone-hooks/) to open the images directly in the Instagram app.
- `{{caption}}` - Image's caption text. Defaults to empty string if there isn't one.
- `{{likes}}` - Number of likes the image has.
- `{{comments}}` - Number of comments the image has.
- `{{location}}` - Name of the location associated with the image. Defaults to empty string if there isn't one.
- `{{model}}` - Full JSON object of the image. If you want to get a property of the image that isn't listed above you access it using dot-notation. (ex: `{{model.filter}}` would get the filter used.)

As of __v1.4.0__, Instafeed.js includes several helpers you can use in your `template` option
to work with the new image sizes. These helpers are meant primarily to help control styling
of the images through CSS.

- `{{width}}` - contains the image's width, in pixels
- `{{height}}` - contains the image's height, in pixels
- `{{orientation}}` - contains the image's orientation. Can be `square`, `portrait`, or `landscape`.
