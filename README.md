# hugo-photoswipe5-gallery

This is an automagical css image gallery theme for [Hugo](https://gohugo.io/) using shortcodes, with an optional lightbox/carousel gadget. For this [PhotoSwipe 5.3.x](http://photoswipe.com/) and [Dynamic caption plugin](https://github.com/dimsemenov/photoswipe-dynamic-caption-plugin) are used.

This theme can create a gallery of all images in a [page bundle](https://gohugo.io/content-management/page-bundles/). It uses [Hugo Page Resources](https://gohugo.io/content-management/page-resources/), which allows to create thumbnails on the fly, with a configurable thumbnail size.

## Demo

- Real-life example at https://www.thkukuk.de/gallery/
- Feature demonstration at https://www.thkukuk.de/blog/hugo-photoswipe5-gallery/

## Image Gallery Features

- A `{{< gallery />}}` shortcode which will generate a gallery of all images in that directory
  - Gallery is responsive, images are scaled/cropped to fill square (or other evenly-sized) tiles
  - Pretty captions appear/slide/fade upon hovering over the image
  - Optionally make gallery images zoom, grow, shrink, slide up, or slide down upon hover
- New `{{< picture >}}` shortcode that is similar to Hugo's built-in `{{< figure >}}`shortcode works together with `gallery` shortcode.
  - Use the `{{< picture >}}` shortcode by itself to enable pretty captions
  - Put multiple `{{< picture >}}` shortcodes inside a `{{< gallery >}}` to create a pretty image gallery
- If you add `{{< load-photoswipe >}}`, the images will be shown in a lightbox/carousel style image gallery
- CSS is automatically loaded the first time you use the `{{< picture >}}` shortcode on each page
- Sidecar files for additonal meta data

## PhotoSwipe Features

- Load PhotoSwipe by calling the `{{< load-photoswipe >}}` shortcode anywhere in your post
- Loads all of the `{{< picture >}}` elements in your post, regardless of where in your post they appear, into a lightbox/carousel style image gallery
- Loads PhotoSwipe and the danamic caption plugin js and css libraries from the local server, so GDPR conform no meta data can be transmitted to a 3rd party hoster.

## Installation

Check out this repo into your `themes/` folder:

```
git submodule add git@github.com:thkukuk/hugo-photoshop5-gallery.git themes/hugo-photoshop5-gallery
```

Then update your `./config.toml` to load the theme, for example:

```
theme = ["hugo-main-theme", "hugo-photoshop5-gallery"]
```

## `{{< picture >}}` shortcode usage

Specifying your image files:

- `{{< picture src="image.jpg" >}}` will use `image.jpg` for lightbox and create a thumbnail from it.

Optional parameters:

- All the [features/parameters](https://gohugo.io/extras/shortcodes) of Hugo's built-in `figure` shortcode work as normal, i.e. src, link, title, caption, class, attr (attribution), attrlink, alt
- `thumbnail-size` sets the size of the thumbnail. Default is "300x300". First number is width, second number is height.
  - example: `{{< picture src="image.jpg" thumbnail-size="150x150" />}}`

Optional parameters for standalone `{{< picture >}}` shortcodes only (i.e. don't use on `{{< picture >}}` inside `{{< gallery >}}` - strange things may happen if you do):

- `caption-position` and `caption-effect` work the same as for the `{{< gallery >}}` shortcode (see below).
- `width` defines the [`max-width`](https://www.w3schools.com/cssref/pr_dim_max-width.asp) of the image displayed on the page. If using a thumbnail for a standalone figure, set this equal to your thumbnail's native width to make the captions behave properly (or feel free to come up with a better solution and submit a pull request :-)). Also use this option if you don't have a thumbnail and you don't want the hi-res image to take up the entire width of the screen/container.

## `{{< gallery >}}` shortcode usage

To specify a directory of image files:

```
{{< gallery />}}
```

- The images are automatically captioned with the file name.
- Thumbnails are automatically generated

To specify individual image files:

```
{{< gallery >}}
  {{< picture src="image1.jpg" >}}
  {{< picture src="image2.jpg" >}}
  {{< picture src="image3.jpg" >}}
{{< /gallery >}}
```

## Optional parameters:

* `caption-effect` - determines if/how captions appear upon hover. Options:
  * `slide` (default)
  * `fade`
  * `none` (captions always visible)
* `caption-position` - determines the captions’ position over the image. Options:
  * `bottom` (default)
  * `center`
  * `none` hides captions on the page (they will only show in PhotoSwipe)
* `hover-effect` - determines if/how images change upon hover. Options:
  * `zoom` (default)
  * `grow`
  * `shrink`
  * `slideup`
  * `slidedown`
  * `none`
* `hover-transition` - determines if/how images change upon hover. Options:
  * not set - smooth transition (default)
  * `none` - hard transition
* `thumbnail-size` sets the size of the thumbnails for the gallery. Default is "300x300". First number is width, second number is height. This option affects the quality and size of the preview image, but not the display size, which depends on the style.
  * example: `{{< gallery thumbnail-size="150x150" />}}`
* `match` - limits the search path and pattern for images
* `sort` - decides how the images get's sorted. Options:
  * `asc` (ascending, default)
  * `desc` (descending)

## Global Configuration Variables:

```
[params.gallery]
  useExif = true
  sort = asc
```

* `useExif` defines if Exif informations should be shown in the photoswipe lightbox. This can be overwritten per page in the [Front Matter](https://gohugo.io/content-management/front-matter/).

* `sort` decides whether the images are sorted ascending ("asc") or descending ("desc") and can be overwritten per `gallery` option.

## Sidecar files

The metadata embedded in an image can be extended or overwritten by a metadata sidecar file. The file must have the same name as the image plus ".meta" (e.g. "image.jpg.meta"). The content has to be a JSON like:

```
{
"Tags": ["amrum","natur"],
"Title": "Lighthouse",
"Caption": "The Lighthouse of Amrum",
"Rating": 2
}
```

## PhotoSwipe usage

- Call `{{< load-photoswipe >}}` **once** on each page where you want to use PhotoSwipe.
- It doesn't matter where on the page.
- If you don't load PhotoSwipe, each figure will instead have a good ol' fashioned hyperlink to a bigger image (or - if you haven't specified a bigger image - the same one).

## CSS Hackers

`hugo-easy-gallery.css` is designed to provide square tiles in a container with `max-width: 768px`.

Here are some pointers if you want to adapt the CSS:

 - Change `.gallery {max-width: 768px;}` if you want a gallery wider than 768px.
 - Change `min-width` in the `@media` styles to change the screen widths at which the layout changes
 - If you want more than 4 tiles per row, set `width` = 100% / number of tiles per row
 - `padding-bottom` = `width` gives square tiles. Change padding-bottom if you want some other aspect ratio, e.g. `width: 33.3%; padding-bottom: 25%` gives a 4:3 aspect ratio.

## Issues

I've tested this with my [hugo-personal-home](https://github.com/thkukuk/hugo-pesonal-home/) theme. If things don't work properly with other themes, raise an issue on GitHub, or even better fix the issue and submit a pull request :-)

## Credits

Thanks to [Li-Wen Yip](https://www.liwen.id.au/) for his [easy-gallery](https://github.com/liwenyip/hugo-easy-gallery) theme, which inspired me a lot and where my theme is based on.
