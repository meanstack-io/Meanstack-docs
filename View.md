# View
*Handlebars.js, view, partials and layout*

---

- [**Introduction**](#introduction)
- [**Creating Views**](#creatingviews)
- [**Creating layouts**](#creatinglayouts)
- [**Creating partials**](#creatingpartials)
  - [**Adding partials to the layout**](#addingpartialstothelayout)
- [**Serving the SPA index**](#servingthespaindex)

---

## Introduction

To manage the view we use the [Handlebars.js](http://handlebarsjs.com/). Possible to use layouts call partial and helpers.

## Creating Views
The Views are in "resources/views" previously set to "config/view.js" => "path".
Create a file in "resources/views/" with the desired name ".hbs", example "greeting.hbs"

```html
<section>
    <h1> Hellow !!! </h1>
</section>
```

## Creating layouts

To assist the development we work with layout. The Layouts are in "resources/views/layouts/" previously set to "config/view.js" => "layoutsDir".

Create a file in "resources/views/layouts/" with the desired name ".hbs", example "app.hbs"

```html
<!DOCTYPE html>
<html ng-app="App">

<head>
    <meta charset="utf-8">
    <link href="{{style 'style'}}" rel="stylesheet" type="text/css">
</head>

<body>

{{{body}}} <!-- Content injected here -->

<script src="{{script 'dependencies'}}" type="text/javascript"></script>
<script src="{{script 'angular'}}" type="text/javascript"></script>

</body>
</html>
```
*{{script 'dependencies'}} and {{style 'style'}} They are helpers of Handlebarsjs, responsible for adding the file path.*

## Creating partials

To compose blocks and facilitate our development we use partials. The Partials are in "resources/views/partials/" previously set to "config/view.js" => "partialsDir".

Create a file in "resources/views/partials/" with the desired name ".hbs", example "footer.hbs"

```html
<footer>
    This is a footer.
</footer>
```

### Adding partials to the layout

```html
<body>

{{{body}}}

{{>footer}} <!-- Added partial footer-->

</body>
```

## Serving the SPA index
*SPA(Single Page Application) index - AngularJS*

The index of a SPA is the main page of your application in it that your application will be loaded.

Created a file in "resources/views/" with the name "index.hbs".

```html
{{!< app}}

<main ui-view></main>
```

We are adding the "app" layout we created earlier, and adding the "ui-view" directive where we will inject our content.


## Next steps

* Routes - AngularJS
* Routes - Express
