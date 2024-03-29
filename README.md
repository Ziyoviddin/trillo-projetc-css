# Trillo: A Flexbox Project

This is the second project from the [Jonas Schmedtmann's Advanced CSS and Sass course.](https://www.udemy.com/advanced-css-and-sass/ "Udemy | Advanced CSS and Sass: Flexbox, Grid, Animations and More!") Main goal of this project was to build a responsive landing page by using the **Flexbox CSS layout method**.

Besides flexbox, the course covers the usage of SVG icons, BEM methodology, CSS variables, animations and transitions as well as responsive design, some handy CSS tricks and helpful online tools.

View project live at [https://trillo-projetc-css.vercel.app/](https://trillo-projetc-css.vercel.app/).


## Instructions

Clone or download project and then run:

```
npm install
npm start
```

*Notice: Project is set to open on `localhost:8080` address in the default browser by default. You can change any of these settings in `package.json` under `devserver` property. If you have any troubles with starting the project please remove all the flags. [See more live-server config options here.](https://github.com/tapio/live-server#usage-from-command-line)*


## Table of Contents

- [Flexbox Layout](#flexbox-layout)
- [CSS Custom Properties](#css-custom-properties)
- [SVG Icons vs. Icon Fonts](#svg-icons-vs-icon-fonts)
- [Responsive Design](#responsive-design)
- [NPM and NPM Scripts](#npm-and-npm-scripts)
- [Cool Tools](#cool-tools)


## Flexbox Layout

Flexbox is a one-dimensional layout method for laying out items in rows or columns. The main idea behind the flexbox is to give the container, a parent element with `display: flex` property the ability to expand children elements to fill available free space or shrink them to prevent overflow, and thus best utilize the available space. Also, children elements order can easily be rearranged by using the CSS `order` property.

As stated on the [MDN Flexbox Page](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Flexbox), some of the best use cases of flexbox layout are:

> - Vertically centering a block of content inside its parent.
> - Making all the children of a container take up an equal amount of the available width/height, regardless of how much width/height is available.
> - Making all columns in a multiple-column layout adopt the same height even if they contain a different amount of content.

So, besides vertically and horizontally centering items, flexbox can be used to easily build responsive multi-column lists by utilizing `flex-wrap` property and setting list items width:

![Flexbox: multi-column list](img/readme/multi-column-list.gif "Responsive multi-column list")

Here we can also see a clever use of CSS `mask-image` property to display colored list item marker (of course, [not supported in IE browsers](https://caniuse.com/?search=mask-image "Can I use mask-image?")). Btw, we can always create a fallback option by using **CSS Feature Queries** `@supports` to check if the browser supports selected properties.

Flexbox is supported in all major browsers except IE 9 and lower. *[See browser support for flexbox.](https://caniuse.com/?search=flexbox "Can I use flexbox?")*


## CSS Custom Properties

>**Custom** properties (sometimes referred to as **CSS variables** or **cascading variables**) are entities defined by CSS authors that contain specific values to be reused throughout a document.
*[Source: MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties "Using CSS custom properties (variables)")*

They are usually defined in the `:root` CSS pseudo-class which represents the `<html>` element and is identical to the selector `html`, except that its specificity is higher.

```scss
:root {
  --color-primary: #eb2f64;
  --color-primary-light: #FF3366;
  --color-primary-dark: #BA265D;

  --color-grey-light-1: #faf9f9;
  --color-grey-light-2: #f4f2f2;
  --color-grey-light-3: #f0eeee;
  --color-grey-light-4: #ccc;

  --color-grey-dark-1: #333;
  --color-grey-dark-2: #777;
  --color-grey-dark-3: #999;

  --shadow-dark: 0 2rem 6rem rgba(0,0,0,.3);
  --shadow-light: 0 2rem 5rem rgba(0,0,0,.06);

  --line: 1px solid var(--color-grey-light-2);
}
```

We can easily access those CSS variables anywhere by using the `var()` function: `color: var(--color-primary);`

Some of the main reasons to use native CSS variables instead of SASS variables as mentioned on [CSS-TRICKS website](https://css-tricks.com/difference-between-types-of-css-variables/ "What is the difference between CSS variables and preprocessor variables?") include:

> - You can use them **without the need of a preprocessor**.
> - **They cascade.** You can set a variable inside any selector to set or override its current value.
> - When their values change (e.g. media query or other state), **the browser repaints as needed**.
> - You can access and **manipulate them in JavaScript**.

Another interesting article on this topic by codyhouse.co: [Why we prefer CSS Custom Properties to SASS variables](https://codyhouse.co/blog/post/css-custom-properties-vs-sass-variables).

CSS variables are not supported in IE browsers. *[See browser for CSS variables.](https://caniuse.com/?search=variables)*


## SVG Icons vs. Icon Fonts

One major advantage of SVG icons over Icon fonts is their superior accessibility. They have a lot of built in elements like `<title>` and `<desc>` which makes them accessible to screen readers. Also, SVGs allow us to have multicolored icons and wide variety of animation options. SVGs are treated as images by browsers and are easier to position. On the other hand, Icon fonts are inserted using pseudo-elements and their positioning is sometimes challenging. They may also encounter occasional failures. However, Icon fonts enjoy better cross browser compatibility. See more in [this great article by LambdaTest](https://www.lambdatest.com/blog/its-2019-lets-end-the-debate-on-icon-fonts-vs-svg-icons/) or [TLDR version on CSS-Tricks website](https://css-tricks.com/icon-fonts-vs-svg/).

This project uses SVG sprite generated through [icomoon.app](https://icomoon.io/). SVG icons are then easily displayed with the help of `<use>` element:

```html
<svg>
  <use xlink:href="img/sprite.svg#icon-star"></use>
</svg>
```

Also, one neat trick when using SVG icons in navigation links is that we can set their `fill` property to `currentColor`, which means that they inherit the color of the parent element. When parent color changes, SVG color changes as well:

![Current Color: Hover example](img/readme/current-color.gif "Current Color: Hover example")

*Notice: Original code changed for the sake of the example.*

SVG is supported in all major browsers and IE9+ (with a couple of known issues). *[See browser support for SVG.](https://caniuse.com/?search=svg)*


## Responsive Design

This project utilizes **Desktop First** strategy. This means that we first write CSS for desktop screens and then use media queries (`@media`) to adapt CSS for smaller tablet and mobile screens. Also, we are not using some specific breakpoints, e.g. like Bootstrap does (`576px, 768px, 992px, 1200px`), but instead we add media queries when design starts to break. *This approach is not recommended for bigger projects.*

Flexbox allows us to very easily make changes in a website layout. Take for example the main navigation, displayed in the sidebar section for desktop screens and in the header section for tablet and mobile screens. This is done by setting the parent element `flex-direction` from `row` to `column`. Easy. Right?

| Desktop Screens                             | Tablet & Mobile Screens                     |
|:-------------------------------------------:|:-------------------------------------------:|
| ![](img/readme/desktop-screens.png)         | ![](img/readme/tablet-mobile-screens.png)   |


## NPM and NPM Scripts

This project relies on the raw power of `npm` (Node Package Manager) to manage dependencies and create build processes. Besides aforementioned `start` script - which runs live-server and compiles SCSS in parallel, we also have a `build:css` which does the following actions:

1. Compiles SASS (SCSS)
2. Uses PostCSS Autoprefixer feature to apply prefixes for better browser support
3. Compresses (minifies) CSS


## Cool Tools

Here is the list of some cool tools used throughout this course:

- [Icomoon](https://icomoon.io/): A great library of free vector icons packs. This app allows us to generate icon fonts from icons available on website as well as from custom icons uploaded to the app.
- [Cubic Bezier](https://cubic-bezier.com/): A tool used to easily generate desired transition values for `cubic-bezier()` function.
- [HTML Glyphs](https://css-tricks.com/snippets/html/glyphs/): A list of glyphs and their corresponding codes that can be used in HTML and CSS files.
- [BEM Methodology](http://getbem.com/): A CSS methodology that helps us to easier organize our CSS code and prevent headaches caused by thinking about how to name a CSS class.
- [Can I Use](https://caniuse.com/): A tool which provides us with up-to-date browser support tables for support of front-end web technologies on desktop and mobile web browsers.
- [Tiny PNG](https://tinypng.com/): A tool for smart PNG and JPEG compression
- [Screen To Gif](https://www.screentogif.com/): Screen, webcam and sketch-board recorder with an integrated editor
- [Photopea](https://www.photopea.com/): Free online photoshop :trollface:

---

### Additional Challenges

- [X] Display a menu with actions when user hovers over user icon
- [X] Display a menu with messages when user hovers over message icon
- [X] Display a menu with bookmarks when user hovers over bookmark icon
- [ ] Display a search suggestions menu when typing in search input
- [X] Create captions for gallery images
- [X] Make the page 100% responsive (`< 500px`)
- [X] Optimize gifs and images
- [ ] Divide components.scss to multiple files
- [ ] Create separate version of user navigation for smartphones and tablets
- [X] Add preloader


---


### License
Copyright &copy; 2023 by Ziyoviddin. **All rights reserved.**
