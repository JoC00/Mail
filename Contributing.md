Looking to improve Antwort? Please read this page carefully before looking to contribute.

### Antwort is partially generated code

Please note Antwort uses template generated code, even before the `build.html` file is generated. Using a generator system allows for shared code between multiple templates for faster development.

##### Quirks, known issues

- Spacing and indenting is not uniform.
- Multiple occurrences of media query breakpoints, e.g. `@media screen and (max-width: 599px)` in `build.html` 
  This is not efficient but a result of a shared responsive css across all templates and a template-specific responsive styles.

##### Where is the generator code?

This is not included because the Antwort open source templates are about learning how to code responsive layouts for Email. Therefore I want to keep the markup as pure HTML and CSS and workflow agnostic. 

## Where can I contribute changes?

You can contribute and improve Antwort by making changes to the source `[TEMPLATE_NAME].html` file. 

Be sure to read about the known issues above. Changes related to spacing and said duplicate media query breakpoints may not be excepted.

## How to contribute

1. Please make your changes to a separate branch.
2. If you are making a significant change, please run a [Litmus](http://litmus.com) or [Email on Acid](http://www.emailonacid.com/) test and include a link to the results in your pull request. Untested changes are less likely to be merged.
3. Submit pull request.