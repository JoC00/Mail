# Getting Started with Antwort

Antwort is not *really* a framework. It is meant to teach you how to do responsive emails, by transforming columns into rows.

## Organization

Download the templates by cloning the repository. Currently we have three (3) available templates:

- single column fluid layout
- two column simple layout
- three column with images

Each template has the following layout:

    build.html
    source/
      |- styles.css
      |- responsive.css
      |- [template_name].html


* `build.html` - includes the inlined CSS. Use this markup with testing the template via an ESP.
* `styles.css` - template specific CSS.
* `responsive.css` - template specific mobile styles. Note that the mobile styles
    * includes reset styles to normalize how clients render email.
    * uses preferred `[class*="selector"]` syntax to prevent Yahoo from rendering mobile version on desktops.


## Recommended Workflow

1. Edit the source html as you see fit.
2. Re-inline the CSS as you see fit.


