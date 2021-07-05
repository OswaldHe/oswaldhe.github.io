---
layout: single
title:  "Add Custom Jekyll Theme"
category: "dev_note"
last_modified_at: "2021-06-13"
---

Currently, github page only support a few types of Jekyll theme on [this page](https://pages.github.com/themes/). You can add these themes using gem style installation in `Gemfile` or directly on the website of your github repo. Things is a little different when using custom theme like [minimal mistake](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/). The following warning will pop up and your deployed page will become blank:
```
The page build completed successfully, but returned the following warning for the `master` branch:

You are attempting to use a Jekyll theme, "minimal-mistakes-jekyll", which is not supported by GitHub Pages. Please visit https://pages.github.com/themes/ for a list of supported themes. 

If you are using the "theme" configuration variable for something other than a Jekyll theme, we recommend you rename this variable throughout your site. For more information, see https://docs.github.com/github/working-with-github-pages/adding-a-theme-to-your-github-pages-site-using-jekyll.

```
This is because github page does not support gem-based method for installation of custom theme. To fix this issue, use remote theme method:
```yaml
remote_theme: "mmistakes/minimal-mistakes@4.23.0"
```
in your `_config.yml` and remove `theme` attribute from it.

Notice that modifying global scss variables in `/_scss/minimal-mistakes/_variables.scss` does not work
for remote theme anymore since bundle pulls all css information under the folder `/_scss` from a remote server without checking whether there is a change of those files locally. In order to change some styling globally, like font type or font size, the easiest option is adding the file `/assets/css/main.scss`. Jekyll will override the styling in this file after pulling scss files from the remote theme so that styles are cascaded.

To write scss files and override global constants, you need the front matter (it can be any text, just for recognizing that this file is the `main.scss` that we want to override) and import all styling of minimal mistakes
```scss
@import "minimal-mistakes/skins/{{ site.minimal_mistakes_skin | default: 'default' }}"; // skin
@import "minimal-mistakes"; // main partials
```
Add styling under `html` tag so that it can be applied globally. To give customization for different types of text (header, descriptions, contents), use breakpoints as it is used originally by minimal mistakes. Here is an example to change font size for different kind of texts:
```scss
@include breakpoint($medium) {
    font-size: 15px; 
}

@include breakpoint($large) {
    font-size: 17px; 
}

@include breakpoint($x-large) {
    font-size: 18px; 
}
```
These breakpoints are built-in in Minimal Mistakes. The list of all breakpoints are over [here](https://github.com/mmistakes/minimal-mistakes/blob/master/_sass/minimal-mistakes/_variables.scss). I would recommend to take a look at how it works in this [blog](https://medium.com/codeartisan/breakpoints-and-media-queries-in-scss-46e8f551e2f2) so that you can get a better idea about how to use it for media query.