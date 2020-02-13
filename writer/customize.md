# Customizing CSS

If you can write CSS, you can customize the appearance of your WriteFreely blog.

<!--more-->

## Getting Started

All you need to do is go to the _Customize_ settings of your blog. Scroll down to "Custom CSS" and customize your blog from there.

The following stylesheet shows a few basic selectors you'll need in order to customize certain elements. In the BASICS section, just grab the selectors (e.g. ```#blog-title a:hover```) for your stylesheet — the properties are only there to illustrate what you can do.

You should copy entire sections in the RECIPES section verbatim, as each is a complete customization you might want to make, like centering an image.

```CSS
/*

    BASICS
    Use the following CSS rules to change the elements you're trying to customize.

*/

body {
    background-color: #efefef;
}

/* Blog header on index and post pages */
#blog-title a {
    color: #fff;
    background-color: #7a629d;
}
#blog-title a:hover {
    color: #eee;
    background-color: #7a629d;
}

/* Blog header on post pages ONLY */
body#post #blog-title a {
    padding: 4px 8px;
}

/* Blog description (underneath title) on index page */
header p.description {
    font-style: italic;
}

/* Post titles on blog index */
.post-title {
    font-weight: normal;
}
.post-title a.u-url:link, .post-title a.u-url:visited {
    color: blue;
}

/* "Read more..." links */
body#collection a.read-more {
    text-decoration: underline;
}

/* Links inside blog posts */
article p a {
    color: #444;
    text-decoration: none;
    border-bottom: 2px solid orangered;
}
article p a:hover {
    background-color: orangered;
    color: white;
    text-decoration: none;
}

/*

    RECIPES
    These are common patterns you may want to use on your blog.

*/

/* Center images */
img {
    display: block;
    margin: 0 auto;
}

/* Disable post header fade effect */
body#post header
    -moz-opacity: 1;
    -khtml-opacity: 1;
    -webkit-opacity: 1;
    opacity: 1;
}

/* Hide post views */
header nav .views {
    display: none;
}

```

## Themes

If you want to see how you can customize beyond the basic elements of your blog, you can check out our [Themes blog](https://write.as/themes). Here, you can find custom blogs from the WriteFreely/Write.as community whose themes you can use & remix for your own blog.
