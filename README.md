# qrparking

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

## Create a github pages build

create a new file

```sh
touch vue.config.js
```

witht the following content

```js
module.exports = {
  publicPath: "/qrparking/",
  outputDir: "dist",
};
```

## Canvas

Documentation - https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D