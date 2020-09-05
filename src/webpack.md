# Webpack

Webpack is a module compiler and bundler.

## General

- Create-React-App comes with Webpack already set up.
- Webpack compiles/transforms vanilla code and bundles multiple files into one.
- It usually outputs one bundle file, but this can be configured to produce more than one.
- Webpack automatically adds `<script>` tag in html, linking to the auto-generated bundled files.
- `webpack.config.js` in root directory is used to configure everything. Inside it, import all required modules and export everything:

  ```
  import '...'

  module.exports = {
    ...
  }
  ```

## Essential Parts

### NPM

- Install Webpack: `npm i webpack webpack-cli`
- Add to `package.json`:

  ```
    "scripts": {
      "build": "webpack"
    },
  ```

- Use script `webpack -w` to watch for all file changes.
- Running `webpack -p` will switch to production build, which minifies bundled code.
- `npm i webpack-dev-server` is used to starts a dev server and open the application in browser.
- For more required/useful modules, loaders, and plugins, see [this repository](https://github.com/Jack4125/react-starter).

### Entry Point

- Inside `webpack.config.js`, include the line:  
  `entry: './index.js',`

### Output

- ```
  output: {
      path: path.resolve(__dirname, 'dist'),
      filename: 'index_bundle.js'
  },
  ```
- If using `path.resolve`, remember to `import 'path'`.

### Loaders

- Loaders tell Webpack _what files_ need to be transformed _using what module_.
- ```
  module: {
    rules: [
      { test: /\.js$/, exclude: /node_modules/, use: "babel-loader" }
      { test: /\.coffee$/, use: "coffee-loader" }
    ]
  },
  ```

### Plugins

- ```
  plugins: [
    new HtmlWebpackPlugin({
      template: 'app/index.html'
    })
  ],
  ```
- [Difference between Loaders and Plugins](https://stackoverflow.com/questions/37452402/webpack-loaders-vs-plugins-whats-the-difference)

### Mode

- `mode: "development"` or `mode: "production"` must be defined.
- This will enable things like tooling for debugging and faster builds.

## Sources

- Webpack [Concepts](https://webpack.js.org/concepts) or [Getting Started](https://webpack.js.org/guides/getting-started/)

- If polyfill is needed (for async, await, etc.), see [this video](https://www.youtube.com/watch?v=iWUR04B42Hc) at 24:55 mark.

## Webpack for Backend?

- See [this](https://stackoverflow.com/questions/37788142/webpack-for-back-end) post.
