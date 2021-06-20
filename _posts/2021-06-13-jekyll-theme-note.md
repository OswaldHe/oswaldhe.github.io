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