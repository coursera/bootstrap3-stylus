# Bootstrap 3.1.1 Stylus 

A basic port of the Twitter Bootstrap CSS framework to stylus, specifically Bootstrap 3.1.1. We only include the .styl files here. Please obtain the js/ and fonts/ from the original source if you need those (https://github.com/twbs/bootstrap). Note that you need Stylus version 0.42.3 or higher.

To learn more about using the Bootstrap framework see the bootstrap docs at http://getbootstrap.com/.

## Usage
Import in your .styl files like this (or replace with relative path of wherever the bootstrap3-stylus/styl/bootstrap.styl is located):

```css
@import bootstrap
````

## Notes on Conversion
This port was created using the less2stylus converter available here: https://github.com/andreypopp/less2stylus. However, I had so make several modifications to the converter, as well as the resulting stylus code in order for stylus to compile as well as to ensure that the resulting output of .less and .styl files was the same. I have published the changes to the converter here: https://github.com/coursera/less2stylus

These are some differences between the less->css and stylus->css versions which could not be removed:
* There are some slight color differences due to the differences between the color functions in LESS and those in Stylus
* Stylus compiled versions add -webkit box-sizing & -moz-boz-sixing whenever applicable. Should not hurt.
* Percentages in stylus versions have more significant digits than less versions.
* @-ms-viewport gets compiled into -ms-viewport in stylus, its a known bug: https://github.com/LearnBoost/stylus/issues/1322

Apart from that, I had to make several manual edits to the generated .styl files to make them same as the less versions:
* Mixin conversion
    - Some of the mixins used for grids were using less conditionals which were not translated to stylus conditionals. I manually rewrote those mixins. E.g. make-grid-columns, float-grid-columns etc
    - Some mixins were not correctly using interpolation for selector generation. I manually edited to use {} for that. (specifically table-row-variant in mixins.styl)
    - A lot of classes in .less files were used as both mixins as well as classes. This does not work in stylus. For these classes I manually converted the class definition to mixin, and then used that both to create the class as well as to be used as mixin later on in other files. E.g. list-unstyled in type.styl, dropdown-menu-right in dropdown.styl, input-lg, input-sm, .nav-justified, .pull-left, .pull-right, 
    - 'clearfix()' mixin was instantiated in mixins.styl so that it can be used in that same file
* Many properties such as box-shadow, box-sizing were both used as mixins and properties in the .less files. This is fine in less but in stylus, they were always treated as mixins, which caused issues with their arguments. I manually converted them to be called as mixins always.
* 'fadein' in less is 'fade-in' in stylus, so had to change that. 
* IE filters
    - Added a mixin defintion for argb which does not exist in stylus (needed for IE filters)
    - removed extra e() in IE filters which are not needed in stylus.
* The converter converts 'translate' mixin to 'mixin-translate' to avoid confusion. I manually changed one of the places where this was used.
* There are issues when less variables are used in property values. Such as "@breadcrumb-separator \000". The conversion did not take care of this. So I manually edited the value to be breadcrumb-separator + "\000"
