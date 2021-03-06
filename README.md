# React scoped CSS

CSS encapsulation solution for React

## Why

In order to solve the problem of css encapsulation, there are two main approaches, css-modules and css-in-js. However, both of them have a very very big problem. The developer experience is not good, by which I mean you often have to write more code than you expect to achieve a simple style. With react-scoped-css, you can just write the normal css you know, while having the advantage of css encapsulation!

## How to use

You have to add one babel plugin and one webpack loader.

**the babel plugin**

```
yarn add babel-plugin-react-scoped-css --dev
```

and in your babelrc add

```json
"plugins": ["babel-plugin-react-scoped-css"]
```

If you have other plugins installed, just add it to the list, order doesn't matter.

Note: this plugin accepts `include`(RegExp, which defaults to `/\.scoped\.(s)?css$/`) to config which css file to be identified as scoped.

**the webpack loader**

```
yarn add scoped-css-loader --dev
```

and in your webpack.config.js

```js
{
  test: /\.(s)?css$/,
  use: [
    {
      loader: 'style-loader',
    },
    {
      loader: 'css-loader',
      options: {
        sourceMap: true,
        importLoaders: 2,
      },
    },
    // You have to put in after `css-loader` and before any `pre-precessing loader`
    { loader: 'scoped-css-loader' },
    {
      loader: 'sass-loader',
    },
  ],
},
```

That's it for the configuration.

## Example

```css
/* Title.scoped.css */
.title {
  background: #999;
}

p {
  color: #ddd;
}
```

```js
// Title.jsx
import React from 'react'
import './Title.scoped.css'

const Title = props => {
  return (
    <h1 className="title">
      <p>{props.children}</p>
    </h1>
  )
}

export default Title
```

Then, in the html, component with scoped css file imported has a unique `data-v-<hash>` attribute on the html element tag, and the css selector also has a corresponding hash like `selector[data-v-<hash>]`.

## Some common use cases with react scoped css

Check out [simple-scoped-css-example](https://github.com/gaoxiaoliangz/react-scoped-css/tree/master/examples/simple)

## Use scoped css in create react app

Due to the fact that create react app doesn't allow user to modify the config of webpack or babel, I created my own fork of a configurable version of it. You can find it [here](https://github.com/gaoxiaoliangz/create-react-app/tree/master/packages/react-scripts).

### Use in a new create-react-app project

```
create-react-app app_name --scripts-version=@gxl/react-scripts
```

and you are good to go, react scoped css is supported by default

### Use in an existing create-react-app project (which hasn't been ejected yet)

```
npm install @gxl/react-scripts --save-dev
npm remove react-scripts
```

and next time you start your project you're bootstrapping from @gxl/react-scripts
