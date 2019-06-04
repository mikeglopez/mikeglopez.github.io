---
layout: post
title:  "Webpack Source Mapping for Debugging in Chrome"
date:   2019-06-03 19:12:24 -0600
categories: debugging
featured-img: srcmap4
---
When working with ES6, React, or other code that must be compiled by Webpack, it can be hard debugging from the browser using DevTools because instead of your original code, it displays the compiled and minified code, which can be difficult to read.

If you run into any errors, it's going to point you to a line in that confusing compiled code.

![Google Chrome DevTools error](../assets/img/posts/srcmap4.jpg)


Using source maps solves this problem by allowing you to debug the original code. This maps the lines of code in the compiled file to the corresponding lines in the original file.

I’ll show you how to quickly generate a source map with Webpack for easier debugging in Chrome.

Let’s get started! In Google Chrome, open DevTools by clicking **command + option + i** on mac or **ctrl + shift + i** on Windows, then click on the three vertical dots on the upper right side to bring up the setting menu. Next, click on **"Settings"**.

![Google Chrome DevTools settings](../assets/img/posts/srcmap1.jpg)


In the **"Preferences"** tab, make sure that both **“Enable JavaScript source maps”** and **“Enable CSS source maps”** are ticked.

![Google Chrome DevTools preferences tab](../assets/img/posts/srcmap2.jpg)


To generate source maps with Webpack running in **production**, all you have to do is add the `devtool` and `sourceMapFilename` properties to your **webpack.config.js** file.


```javascript
module.exports = {
  entry: {
    app: `${__dirname}/src/index.jsx`
  },
  output: {
    filename: '[name].js',
    sourceMapFilename: '[name].js.map',
    path: `${__dirname}/dist`
  },
devtool: ‘source-map’
};
```


If you'd like to debug in development, remove and `-d` for **development** or `-p` for **production** flags from your Webpack script and set the `devtool` property in **webpack.config.js** to `eval-source-map`. [Erik Aybar](https://erikaybar.name/webpack-source-maps-in-chrome/)

Next, you need to add a comment with the `sourceMappingURL` to the end of your file to be compiled. This points to the source map.


```javascript
//# sourceMappingURL=/dist/app.js.map
```


When you run your Webpack script, it will compile your files and generate a source map at the destination specified in your **webpack.config.js** file under the output path.


For reference, here is the error without source mapping:

![Google Chrome DevTools error without source map](../assets/img/posts/srcmap4.jpg)


And you'll notice that within the **Page** tab at the top of DevTools, it only shows the compiled file.

![Google Chrome DevTools files without source map](../assets/img/posts/srcmap3.jpg)


And below is what this will look like with source mapping:

![Google Chrome DevTools files with source map](../assets/img/posts/srcmap5.jpg)

![Google Chrome DevTools error with source map](../assets/img/posts/srcmap6.jpg)


Hopefully this is as useful to you as it has been to me!