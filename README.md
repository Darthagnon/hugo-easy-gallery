# hugo-easy-gallery 🖼 
### This fork is currently maintained by [Darthagnon] ([homepage]), associate of Flukey and [Mesh Infrasoft](https://www.meshinfrasoft.com/).

Automagical css image gallery in [Hugo](https://gohugo.io/) using shortcodes, with optional lightbox/carousel gadget using [PhotoSwipe] and **no jQuery** or other frameworks.

## Screenshot

![screenshot](/screenshot.png)

## Demo

- Feature demonstration at https://www.liwen.id.au/heg/
- Real-life example at https://www.liwen.id.au/arduino-rf-codes/
- Production usage by [iroQuai](https://github.com/iroQuai) at https://buurlandutrecht.nl/buurtinbeeld/ ; features a creative example of a grid gallery that links to other pages in Hugo (see also [/exampleSite/buurtinbeeld.md](/exampleSite/buurtinbeeld.md), copied from his [website source](https://raw.githubusercontent.com/iroQuai/buurlandutrecht.nl/main/content/buurtinbeeld/_index.md))
- Production usage by maintainer [Darthagnon] on websites he manages: https://robot-one.github.io/, https://dragonsnotincluded.com/


## Image Gallery Features

- Create a gallery of all images in a directory with just one line of shortcode, see [demo](https://www.liwen.id.au/heg/#gallery-usage): `{{< gallery dir="/img/your-directory-of-images/" />}}`
- Custom `{{< figure >}}` shortcode that enables new features but is backwards-compatible with Hugo's built-in `{{< figure >}}`shortcode
- Use the `{{< figure >}}` shortcode by itself to enable pretty captions
- Put multiple `{{< figure >}}` shortcodes inside a `{{< gallery >}}` to create a pretty image gallery
- **Point `{{< gallery >}}` at a directory to generate a gallery of all images in that directory**
- Gallery is responsive, images are scaled/cropped to fill square (or other evenly-sized) tiles
- Pretty captions appear/slide/fade upon hovering over the image
- Optionally make gallery images zoom, grow, shrink, slide up, or slide down upon hover
- Only requires 3.6kB of CSS (unminified; you can minify it if you want)
- CSS is automatically loaded the first time you use the `{{< figure >}}` shortcode on each page
- **Coming soon:** Automatic thumbnail generation from image resources via [Hugo image processing](https://gohugo.io/content-management/image-processing/)

## PhotoSwipe Features

- Load PhotoSwipe by calling the `{{< load-photoswipe >}}` shortcode anywhere in your post
- Loads all of the `<figure>` elements in your post, regardless of where in your post they appear, into a lightbox/carousel style image gallery
- Works with any existing `<figure>` elements/shortcodes in your posts
- Does not require you to [pre-define the image sizes](http://photoswipe.com/documentation/faq.html#image-size) (the initialisation script pre-loads the image to determine its size; you can optionally pre-define the image size if you want to avoid this pre-loading)
- Loads PhotoSwipe js and css libraries locally or from `cdnjs.cloudflare.com` (specify `CDNJS= true` in `config.toml/[Params]`

## Installation
### As a Theme
**(1)** Check out this repo into your `themes/` folder:

```
git submodule add https://github.com/Darthagnon/hugo-easy-gallery.git themes/hugo-easy-gallery
git submodule init
```

**(2)** Then update your `./config.toml` to load the theme, for example:

```
theme = ["hugo-coder", "hugo-easy-gallery"]
```

### Manual Installation
Click the big green `Code` button at the top of this page, then `Download ZIP` and extract it to your `themes/` folder.

**Or**, put files in following places:

- `/layouts/shortcodes/figure.html`
- `/layouts/shortcodes/gallery.html`
- `/layouts/shortcodes/load-photoswipe.html`
- `/static/js/load-photoswipe.js`
- `/static/css/hugo-easy-gallery.css`

Then, update your `./config.toml` to load the theme, for example:

```
theme = ["hugo-coder", "hugo-easy-gallery"]
```

If you want, you could (depending on a front matter param) conditionally load `load-photoswipe.html` or its contents from the footer of your template.  But I've consciously chosen to load PhotoSwipe using a shortcode so that you don't have to modify your template if you don't want to.

## Theme integration

Feel free to use Hugo Easy Gallery to add Photoswipe to your Hugo themes! Tell us about them so we can list them here! Here's how you do it:

1. Merge the files into the relevant folders within your theme:
	- `myTheme/static/css/{files from hugo-easy-gallery}`
	- `myTheme/static/js/{files from hugo-easy-gallery}`
	- `myTheme/layouts/shortcodes/{files from hugo-easy-gallery}`

2. Move `/layouts/shortcodes/load-photoswipe.html` to `/layouts/partials/load-photoswipe.html`.

3. Add the following line to `myTheme/layouts/_default/baseof.html`, just before the closing `</body>` tag:

```html
	{{ partial "load-photoswipe" }}
</body>
```

Hugo Easy Gallery and Photoswipe will now be loaded on every page on your website; call the `{{< figure >}}` and `{{< gallery >}}` shortcodes anywhere!

## `{{< figure >}}` shortcode usage

Specifying your image files:

- `{{< figure src="thumb.jpg" link="image.jpg" >}}` will use `thumb.jpg` for thumbnail and `image.jpg` for lightbox
- `{{< figure src="image.jpg" >}}` or `{{< figure link="image.jpg" >}}` will use `image.jpg` for both thumbnail and lightbox
- `{{< figure link="image.jpg" thumb="-small" >}}` will use `image-small.jpg` for thumbnail and `image.jpg` for lightbox

Optional parameters:

- All the [features/parameters](https://gohugo.io/extras/shortcodes) of Hugo's built-in `figure` shortcode work as normal, i.e. src, link, title, caption, class, attr (attribution), attrlink, alt
- `size` (e.g. `size="1024x768"`) pre-defines the image size for PhotoSwipe. Use this option if you don't want to pre-load the linked image to determine its size.
- `class` allows you to set any custom classes you want on the `<figure>` tag.

Optional parameters for standalone `{{< figure >}}` shortcodes only (i.e. don't use on `{{< figure >}}` inside `{{< gallery >}}` - strange things may happen if you do):

- `caption-position` and `caption-effect` work the same as for the `{{< gallery >}}` shortcode (see below).
- `width` defines the [`max-width`](https://www.w3schools.com/cssref/pr_dim_max-width.asp) of the image displayed on the page. If using a thumbnail for a standalone figure, set this equal to your thumbnail's native width to make the captions behave properly (or feel free to come up with a better solution and submit a pull request :-)). Also use this option if you don't have a thumbnail and you don't want the hi-res image to take up the entire width of the screen/container.
- `class="no-photoswipe"` prevents a `<figure>` from being loaded into PhotoSwipe. If you click on the figure you'll instead a good ol' fashioned hyperlink to a bigger image (or - if you haven't specified a bigger image - the same one).

## `{{< gallery >}}` shortcode usage

To specify a directory of image files:

```
{{< gallery dir="/img/your-directory-of-images/" />}}
```

**Don't forget the closing forward slash before the end Hugo brackets `/>}}`!!!**

- The images are automatically captioned with the file name.
- `[image].jpg` is used for the hi-res image, and `[image]-thumb.jpg` is used for the thumbnails.
- If `[image]-thumb.jpg` doesn't exist, then `[image].jpg` will be used for both hi-res and thumbnail images.
- The default thumbnail suffix is `-thumb`, but you can specify a different one e.g. `thumb="-small"` or `thumb="_150x150"`.

To specify individual image files:

```
{{< gallery >}}
  {{< figure src="image1.jpg" >}}
  {{< figure src="image2.jpg" >}}
  {{< figure src="image3.jpg" >}}
{{< /gallery >}}
```

Optional parameters:

- `caption-position` - determines the captions' position over the image. Options:
  - `bottom` (default)
  - `center`
  - `none` hides captions on the page (they will only show in PhotoSwipe)
- `caption-effect` - determines if/how captions appear upon hover. Options:
  - `slide` (default)
  - `fade`
  - `none` (captions always visible)
- `hover-effect` - determines if/how images change upon hover. Options:
  - `zoom` (default)
  - `grow`
  - `shrink`
  - `slideup`
  - `slidedown`
  - `none`
- `hover-transition` - determines if/how images change upon hover. Options:
  - not set - smooth transition (default)
  - `none` - hard transition
- `thumbnail-size` sets the size of the thumbnails for the gallery. Default is "300x300". First number is width, second number is height.
  - example: `{{< gallery dir="/img/your-directory-of-images/" thumbnail-size="150x150" />}}`

## PhotoSwipe usage

- Call `{{< load-photoswipe >}}` **once** on each page where you want to use PhotoSwipe.
- It doesn't matter where on the page.
- If you don't load PhotoSwipe, each figure will instead have a good ol' fashioned hyperlink to a bigger image (or - if you haven't specified a bigger image - the same one).

You can optionally have different captions on page vs in PhotoSwipe:

- `{{< figure src="image.jpg" alt="This is a caption">}}` or `{{< figure src="image.jpg" caption="This is a caption">}}` will use the same caption both on the page and in PhotoSwipe.
- `{{< figure src="image.jpg" caption="A short caption" alt="This is a much longer, verbose, comprehensive caption that will be used in PhotoSwipe">}}` will use a different caption in PhotoSwipe.

## CSS Hackers

`hugo-easy-gallery.css` is designed to provide square tiles in a container with `max-width: 768px`.

Here are some pointers if you want to adapt the CSS:

 - Change `.gallery {max-width: 768px;}` if you want a gallery wider than 768px.
 - Change `min-width` in the `@media` styles to change the screen widths at which the layout changes
 - Change `min-width: 9999px` in the last `@media` style to something sensible if you want to use a 4-tile layout
 - If you want more than 4 tiles per row, set `width` = 100% / number of tiles per row
 - `padding-bottom` = `width` gives square tiles. Change padding-bottom if you want some other aspect ratio, e.g. `width: 33.3%; padding-bottom: 25%` gives a 4:3 aspect ratio.

## Issues

This module has been tested with the [beautifulhugo](https://github.com/halogenica/beautifulhugo) theme. If things don't work properly with other themes, raise an issue on GitHub, or even better fix the issue and submit a pull request :-)

### Need help?
- Post your question in **[Issues](https://github.com/Darthagnon/hugo-easy-gallery/issues)**, or contact Darth via his [homepage].
- Include a link to a test page that demonstrates the issue you are having
	- We recommended hosting for free with [GitHub Pages](https://pages.github.com/) and building site based off the Hugo themes [blank](https://github.com/vimux/blank) or [plain](https://github.com/Darthagnon/hugo-plain-theme) with Hugo Easy Gallery added.
- Include your source code for the test page
- Please be patient; the [original project](https://github.com/liwenyip/hugo-easy-gallery) is kinda dead, and we are maintining this part-time...

### Fixed an issue or made an improvement?
- Please submit a **[pull request](https://github.com/Darthagnon/hugo-easy-gallery/pulls)**
- Include a link to a test page
- Include your source code for the test page
- Consider volunteering to help maintain this project :-)

## Credits

These blog posts helped original author Li-Wen Yip immensely:

- http://www.dwuser.com/education/content/creating-responsive-tiled-layout-with-pure-css/
- http://www.thehome.dk/article/photoswipe-gallery-hugo/
- https://webdesign.tutsplus.com/tutorials/the-perfect-lightbox-using-photoswipe-with-jquery--cms-23587

Many thanks to [Li-Wen Yip](https://www.liwen.id.au/), the **original author** of [Hugo Easy Gallery](https://github.com/liwenyip/hugo-easy-gallery), who retired from maintaining it 2022-02-28 due to other commitments and family. Without him, this project wouldn't exist; may he live long and prosper! His blog posts were very informative, and have been included as [exampleSite](/exampleSite) material:
- https://www.liwen.id.au/heg/
- https://www.liwen.id.au/photoswipe/

Thanks to [Kishor Bhat](https://github.com/kishorbhat) and [Emiel Hollander](https://github.com/EmielH), volunteer collaborators.

Thanks to [Dmytro Semenov](https://github.com/dimsemenov), the [PhotoSwipe] author.

Thanks to the [BeautifulHugo theme](https://github.com/halogenica/beautifulhugo) authors, who maintained compatibility and provided example code for this module.

Thanks to [the many community members who have contributed pull requests and issues to the original project](https://github.com/liwenyip/hugo-easy-gallery/pulls) 


[PhotoSwipe]: https://photoswipe.com/
[Darthagnon]: https://github.com/Darthagnon
[homepage]: https://robot-one.github.io/
