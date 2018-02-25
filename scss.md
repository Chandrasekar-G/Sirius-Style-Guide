### 1. Partials

1.1 Partial files are files with name that begin with an underscore. They'll not be compiled to a seperate css file.

Example

>    _mixins.scss

>   _variables.scss

Using partials allow us to better manage the styles by 

    - Breaking the styles into smaller, more managable blocks.
    - Code re-usability
    - Single point of change for modifications

### 2. Variables

2.1 Use variables for all colors, common numbers and numbers with meaning.

```scss
/* colors */
$grey: #F3F3F3; 
$blue: #82D2E5;
$green: #00A878;
$red: #FE5E41;

$link-primary: $orange;
$link-secondary: $blue;
$link-tertiary: $grey;

$success: $green;
$error: $red;

/* constants*/

$zHeader: 2000;
$zOverlay: 5000;
```

### 3. Vendor Prefixes

3.1 Never use vendor prefixes in styles you write. Instead use a third-party library like Bourbon prefixer or write your own mixins to handle them.
