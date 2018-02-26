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

### 4. Mixins and Placeholders

4.1 A mixin could be used if an argument is present, to quickly create modified styles.

```scss
@mixin rounded-corner($arc) {
    -moz-border-radius: $arc;
    -webkit-border-radius: $arc;
    border-radius: $arc;  
}

// USAGE
.tab-button {
     @include rounded-corner(5px); 
}
```
4.2 Placeholders can be used to avoid duplicates. It might look similar to mixins without attributes, but there is a subtle difference between them.

```scss
%bg-image {
    width: 100%;
    background-position: center center;
    background-size: cover;
    background-repeat: no-repeat;
}

.image-one {
    @extend %bg-image;
    background-image:url("/img/image-one.jpg");
}

.image-two {
    @extend %bg-image;
    background-image:url("/img/image-two.jpg");
}
```

The compiled css would look like

```scss
.image-one, .image-two {
    width: 100%;
    background-position: center center;
    background-size: cover;
    background-repeat: no-repeat;
}

.image-one {
    background-image:url("/img/image-one.jpg") ;
}

.image-two {
    background-image:url("/img/image-two.jpg") ;
}
```

If we had used mixins here the compiled css would have had a lot of bloated code.

### 5. Code structuring

Here's a sample code structure. Keep things simple and maintainable. 
```shell
|-- _variables.scss
|-- _mixins.scss
|-- _placeholders.scss
|-- _commons.scss
|-- login.scss
|-- dashboard.scss
vendor/
fonts/
```