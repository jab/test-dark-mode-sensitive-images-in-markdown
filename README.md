# Testing dark-mode-sensitive images in GitHub markdown

* [View source](https://raw.githubusercontent.com/jab/test-dark-mode-sensitive-images-in-markdown/main/README.md)

* Toggle your theme between light mode and dark mode
  while viewing the demos below.


### GitHub's [anchor filter URL syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#specifying-the-theme-an-image-is-shown-to) works:

#### Only shown to GitHub Light Mode users:
![Only shown to GitHub Light Mode users](/light-mode.svg#gh-light-mode-only)

#### Only shown to GitHub Dark Mode users:
![Only shown to GitHub Dark Mode users](/dark-mode.svg#gh-dark-mode-only)


### Inlined `<img src="test.svg">` does not work:

The `@media (prefers-color-scheme: dark)`
selector in the following `test.svg`
to override the black fill color for dark mode users
does not work:

<img src="test.svg">


### Inlined `<svg>` does not work:

GitHub's markdown renderer does not allow `svg` elements,
turning the following `svg` element into a mangled text node
and a `pre`:

<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100" style="background: transparent">
  <style>
    /* assume light theme by default and use black ink */
    circle { fill: #000; }
    /* if dark theme is detected, override with dark-mode styles and use light ink */
    @media (prefers-color-scheme: dark) { circle { fill: #eee; } }

    /* The above doesn't work. How about if we try the following? */
    html[data-color-mode="dark"] circle { fill: #acf; }
    /* This is based on GitHub's html, which looks like the following (simplified):
        〈html data-color-mode="dark"〉〈img src="this.svg"〉...
    This assumes that this CSS-in-SVG can use a selector that matches
    an ancestor element in the containing document and then traverses
    into the contained SVG document to match a descendant element,
    but this doesn't work either. */
  </style>
  <circle cx="50" cy="50" r="50"/>
</svg>
<!-- Can you embed svg elements with no internal style elements? -->
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100" style="background: transparent">
  <circle cx="50" cy="50" r="50"/>
</svg>
