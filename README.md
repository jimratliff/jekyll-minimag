# jekyll-minimag

A work in progress…

A Jekyll site based on the minima theme (version 2.5.1). (The dependence on the gem theme has been overridden, so updates to minima will have no effect on minimag.)

This repo is designed for development within a Docker container.

The initial construction of this site follows Bill Raymond’s YouTube video “[Draft training - Run GitHub Pages in a Docker container](https://www.youtube.com/watch?v=4zCZhPjzlc0&lc=Ugw9B54_UzDEPIQFP_N4AaABAg),” which was a draft and unlisted as of April 2, 2023.

## Features (that differentiate minimag from minima)
### Each blog post is represented on listings pages by a “Card”
One departure of minimag from minima is the implementation of a “card” presentation, including an illustration, for each post’s entry on posts-listing pages. A card has the following elements:
* An image component
* A text component, made up of subcomponents
  * Post title
  * Post excerpt
  * Post date (The last-updated date)
  * Post author

* A “featured post” will have a horizontal card format such that the image will occupy the top of the card, and the text elements will appear below the image
  * All featured-post cards will be the same width, and narrow enough so that on a desktop display, multiple cards will typically span a row of cards for featured posts.
* The posts in a list of not necessarily featured posts will have a vertical card format such that the image will occupy the left part of the card, and the text elements will appear in the right side of the card.
  * Each vertical-format card will span the full width of the display.

### The URLs of posts do *not* encode the post’s date or the post’s category
In its out-of-the-box configuration, the URL for a post `2023-04-02-welcome-to-jekyll.markdown` is simply `/welcome-to-jekyll`.

This is in contrast to the default behavior of, the built-in theme minima, where the URL for a post encodes the post’s date and categories. E.g., when the categories are “jekyll” and “update”: `/jekyll/update/2023/04/02/welcome-to-jekyll.html`.

This decision for minimag was made primarily to allow the date of a post to be updated without changing its URL.

Note: posts now share the same namespace as pages, so there is the possibility of collisions. Names of posts and pages must be chosen to avoid such a collision.



# Usage notes
## Posts
### By default, posts use layout `post.html`
By default (due to a setting in `_config.yml`), every post will inherit the layout `post.html`.

Thus it is not necessary to include `layout: post` in the front matter of a post.

### Images for posts
There are three types of images for posts: (a) `card-image`, (b) `top-image`, and (c) interior images. Types (a) and (b) require special attention in the post’s front matter; type (c) does not.
* `card-image`
  * An image associated with a particular post to be displayed in the *card* for that post on a posts-listing page, such as the home page, a category listing page, or an archive page.
  * To assure uniformity across posts’ cards, there will be a fixed aspect ratio that applies to all posts’ cards and their images.
    * The displayed image will be displayed with `object-fit: cover`, because this (a) does not distort/skew the image’s native aspect ratio and (b) completely fills the alloted space without letterboxing. (See Jim Ratliff, [“How the object-fit CSS property works: Comparing the various values](https://codepen.io/jimratliff/pen/wVYrKb),” CodePen.)
  * To assure uniformity across posts’ cards, an image *will* be displayed in each post’s card, even if the post does not specify a `card-image`.
  * The post-card image displayed depends at least on the value of `card-image`. If `card-image` is
    * specified, the specified image will be displayed
    * `default`, the default post-card image will be displayed
      * Note: Although this is the default behavior even when `card-image` is unspecified, specifying `default` can have an effect when the post *does* have a specification for `top-image`. In this case, specifying `card-image` as `default` prevents the specified `top-image` from being displayed in the post’s card.
    * unspecified:
      * If a `top-image` for the post is specified,
        * that image is used in the post’s card
      * Otherwise:
        * the default post-card image will be displayed
* `top-image`
  * An image displayed at the top of a post.
    * An image in this location requires special attention in the front matter of the post because this image will appear *prior* to the content in the post file. This requires the file name of the image to be passed to `_layouts/post.html` so that the image can be rendered in the header portion of that layout.
  * There is no aspect-ratio constraint for a `top-image`. Its full height will be displayed at the top of a post, and the remaining content of the post will begin immediately below the bottom of the `top-image`.
  * A `top-image` is not mandatory. Thus,
    * If a `top-image` is specified, it is used at the top of the post.
    * If a `top-image` is not specified, an image is not displayed at the top of the post.
* Interior images
  * These are images displayed in the body of a post.
  * These don’t require special attention in the front matter, because the image is rendered within the content of the post’s file.

Usage notes:
* Ideally, whatever image plays the role of `card-image` should have a native aspect ratio close to that of the frame in the card into which it must fit.
* If the same image should be used for both `card-image` and `top-image`, it is sufficient to specify that image for `top-image`, and leave `card-image` unspecified. In that case the image for `top-image` will be chosen for `card-image`.

# License

This site structure is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

# Release history
* v0.0.1, April 2, 2023
  * Essentially identical to the default, built-in minima theme, except that theme is frozen at version 2.5.1.
  * Configured for development within a Docker container.