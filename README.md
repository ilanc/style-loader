[![npm][npm]][npm-url]
[![node][node]][node-url]
[![deps][deps]][deps-url]
[![chat][chat]][chat-url]

<div align="center">
  <a href="https://github.com/webpack/webpack">
    <img width="200" height="200"
      src="https://webpack.js.org/assets/icon-square-big.svg">
  </a>
  <h1>Style Loader</h1>
</div>

Adds CSS to the DOM by injecting a `<style>` tag

<h2 align="center">Install</h2>

```
npm install style-loader --save-dev
```

<h2 align="center">Usage</h2>

[Documentation: Using loaders](http://webpack.github.io/docs/using-loaders.html)

### Simple API

``` javascript
require("style-loader!raw-loader!./file.css");
// => add rules in file.css to document
```

It's recommended to combine it with the [`css-loader`](https://github.com/webpack/css-loader): `require("style-loader!css-loader!./file.css")`.

It's also possible to add a URL instead of a CSS string:

``` javascript
require("style-loader/url!file-loader!./file.css");
// => add a <link rel="stylesheet"> to file.css to document
```

### Local scope CSS

(experimental)

When using [local scope CSS](https://github.com/webpack/css-loader#css-scope) the module exports the generated identifiers:

``` javascript
var style = require("style-loader!css-loader!./file.css");
style.placeholder1 === "z849f98ca812bc0d099a43e0f90184"
```

### Reference-counted API

``` javascript
var style = require("style-loader/useable!css-loader!./file.css");
style.use(); // = style.ref();
style.unuse(); // = style.unref();
```

Styles are not added on `require`, but instead on call to `use`/`ref`. Styles are removed from page if `unuse`/`unref` is called exactly as often as `use`/`ref`.

Note: Behavior is undefined when `unuse`/`unref` is called more often than `use`/`ref`. Don't do that.

### Options

#### `insertAt`

By default, the style-loader appends `<style>` elements to the end of the style target, which is the `<head>` tag of the page unless specified by `insertInto`. This will cause CSS created by the loader to take priority over CSS already present in the target. To insert style elements at the beginning of the target, set this query parameter to 'top', e.g. `require('../style.css?insertAt=top')`.

#### `insertInto`
By default, the style-loader inserts the `<style>` elements into the `<head>` tag of the page. If you want the tags to be inserted somewhere else, e.g. into a [ShadowRoot](https://developer.mozilla.org/en-US/docs/Web/API/ShadowRoot), you can specify a CSS selector for that element here, e.g. `require('../style.css?insertInto=#host::shadow>#root')`.

#### `singleton`

If defined, the style-loader will re-use a single `<style>` element, instead of adding/removing individual elements for each required module. **Note:** this option is on by default in IE9, which has strict limitations on the number of style tags allowed on a page. You can enable or disable it with the singleton query parameter (`?singleton` or `?-singleton`).

#### `convertToAbsoluteUrls`

If convertToAbsoluteUrls and sourceMaps are both enabled, relative urls will be converted to absolute urls right before the css is injected into the page. This resolves [an issue](https://github.com/webpack/style-loader/pull/96) where relative resources fail to load when source maps are enabled.  You can enable it with the convertToAbsoluteUrls query parameter (`?convertToAbsoluteUrls`).

#### `attrs`

If defined, style-loader will attach given attributes with their values on `<style>` / `<link>` element.
Usage:
```javascript
require('style-loader?{attrs:{id: "style-tag-id"}}!style.css');

// will create style tag <style id="style-tag-id">
```
Usage in `url` mode:
```javascript
require('style-loader/url?{attrs:{prop: "value"}}!file-loader!style.css')

// will create link tag <link rel="stylesheet" type="text/css" href="[path]/style.css" prop="value">
```

#### `base`
This setting is primarily used as a workaround for [css clashes](https://github.com/webpack-contrib/style-loader/issues/163) when using one or more [DllPlugin](https://robertknight.github.io/posts/webpack-dll-plugins/)'s.  `base` allows you to prevent either the *app*'s css (or *DllPlugin2*'s css) from overwriting *DllPlugin1*'s css by specifying a css module id base which is greater than the range used by *DllPlugin1* e.g.:
* webpack.dll1.config.js
```
{
  test: /\.css$/,
  use: [ 
    'style-loader',
    'css-loader'
  ]
}
```
* webpack.dll2.config.js
```
{
  test: /\.css$/,
  use: [ 
    { loader: 'style-loader', options: { base: 1000 } },
    'css-loader'
  ]
}
```
* webpack.app.config.js
```
{
  test: /\.css$/,
  use: [ 
    { loader: 'style-loader', options: { base: 2000 } },
    'css-loader'
  ]
}
```

### Recommended configuration

By convention the reference-counted API should be bound to `.useable.css` and the simple API to `.css` (similar to other file types, i.e. `.useable.less` and `.less`).

So the recommended configuration for webpack is:

``` javascript
{
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          { loader: "style-loader" },
          { loader: "css-loader" },
        ],
      },
      {
        test: /\.useable\.css$/,
        use: [
          {
            loader: "style-loader/useable"
          },
          { loader: "css-loader" },
        ],
      },
    ],
  },
}
```

**Note** about source maps support and assets referenced with `url`: when style loader is used with ?sourceMap option, the CSS modules will be generated as `Blob`s, so relative paths don't work (they would be relative to `chrome:blob` or `chrome:devtools`). In order for assets to maintain correct paths setting `output.publicPath` property of webpack configuration must be set, so that absolute paths are generated. Alternatively you can enable the `convertToAbsoluteUrls` option mentioned above.

<h2 align="center">Contributing</h2>

Don't hesitate to create a pull request. Every contribution is appreciated. In development you can start the tests by calling `npm test`.

<h2 align="center">Maintainers</h2>

<table>
  <tbody>
    <tr>
      <td align="center">
        <img width="150 height="150"
        src="https://avatars.githubusercontent.com/sokra?v=3">
        <br />
        <a href="https://github.com/">Tobias Koppers</a>
      </td>
      <td align="center">
        <img width="150 height="150"
        src="https://avatars.githubusercontent.com/SpaceK33z?v=3">
        <br />
        <a href="https://github.com/">Kees Kluskens</a>
      </td>
    <tr>
  <tbody>
</table>


<h2 align="center">LICENSE</h2>

MIT

[npm]: https://img.shields.io/npm/v/style-loader.svg
[npm-url]: https://npmjs.com/package/style-loader

[node]: https://img.shields.io/node/v/style-loader.svg
[node-url]: https://nodejs.org

[deps]: https://david-dm.org/webpack/style-loader.svg
[deps-url]: https://david-dm.org/webpack/file-loader

[chat]: https://badges.gitter.im/webpack/webpack.svg
[chat-url]: https://gitter.im/webpack/webpack
