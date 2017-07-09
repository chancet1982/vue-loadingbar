# vue-loadingbar

> A Vue.js loading-bar component

## Props:
* cssClasses: type: String, required: false
* size: type: String, required: false, default: "1em"
* progress: type: Number, required: false, default: 50,
* posBottom: type: Boolean, required: false, default: false,
* determined: type: Boolean, required: false, default: false,
* color: type: String, required: false, default: 'rgb(205, 27, 106)',
* loading: type: Boolean, required: false, default: true // temp while developing

## Events:
### loading-bar-state-change
This event is fired whenever loader state (loading/ not loading) is changed.

### loading-bar-loading
This event is fired when loader is loading.

### loading-bar-not-loading
This event is fired when loader is not loading.

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For detailed explanation on how things work, checkout the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
