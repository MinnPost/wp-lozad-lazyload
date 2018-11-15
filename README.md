# WordPress Lozad Lazy Load
This is a WordPress plugin built to lazy Load images, iframes, scripts, and other content with the [Lozad library](https://github.com/ApoorvSaxena/lozad.js), which itself uses the IntersectionObserver API.

The way this plugin works is by making a filter available, `wp_lozad_lazyload_convert_html`, which can be applied to markup to produce markup that Lozad can act upon.

## Parameters

The filter accepts two parameters.

The first parameter is `$output_html`. This parameter can be a string - basic HTML - or it can be an array, including a `script` key and a `noscript` key which are both strings of basic HTML. The filter will maintain the type that you send it, so it will return transformed HTML, or the same array structure with transformed HTML.

The filter also accepts a `$params` parameter, an array. You should at least include an `html_tag` key in this array, which should be the tag you're trying to transform. Currently this filter supports `img`, `iframe`, and `script` tags.

If you apply this filter on an HTML tag that is not supported, or by accidentally omitting the `html_tag` key in the array, the filter will return the same HTML that you sent to it.

## Examples

### Image

If you have an image tag, perhaps in a post thumbnail or other attachment markup, it might look like this:

```html
<img src="yourimage.jpg" class="attachment-feature-large size-feature-large" alt="your alt text" srcset="400w-image.jpg 400w, 130w-image.jpg 130w" sizes="(max-width: 400px) 100vw, 400px" />
```

If this image tag is stored in the `$image` variable, you can apply the filter to it like this:

```php
$params = array( 'html_tag' => 'img' );
$image = apply_filters( 'wp_lozad_lazyload_convert_html', $image, $params );
```

The return value will look like this:

```html
<noscript>
    <img src="yourimage.jpg" class="attachment-feature-large size-feature-large" alt="your alt text" srcset="400w-image.jpg 400w, 130w-image.jpg 130w" sizes="(max-width: 400px) 100vw, 400px"></noscript>
<img class="attachment-feature-large size-feature-large lazy-load" alt="your alt text" sizes="(max-width: 400px) 100vw, 400px" data-src="yourimage.jpg" data-srcset="400w-image.jpg 400w, 130w-image.jpg 130w">

```
