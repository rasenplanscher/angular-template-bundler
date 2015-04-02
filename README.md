# angular-template-bundler

> Get your initial view rendered and load all other templates at once

## What it does

Using templateBundler, your initial view will be rendered as usual, but after that all your templates will be fetched in a single HTTP request, so as to reduce overall load without delaying the initial view


## Getting started

### Integrating the code
Install this via your `bower.json` and include `template-bundler.js` with your script files, like so:

```html
<script src="bower_components/angular-template-bundler/template-bundler.js"></script>
```

Require `templateBundler` as a dependency for your app module, like so:

```js
var myApp = angular.module('myApp', ['templateBundler'])
```

### Providing the templates
Make your templates available as a single json file.

In order to make this simple and reliable, simply go to your app directory and execute `build-template-bundle.js`. It will gather all non-hidden files (files not starting with a period character, so as to ignore .gitkeep and the likes) from the `templates/` directory and write them to the file `templates.json`.

If your templates reside in a different directory, you can give that as the first parameter, like so:

```sh
bower_components/angular-template-bundler/build-template-bundle.js tmpl
```

The name of the directory will be used as the file name, so in the above example, the file would be `tmpl.json`. You can override that as well by appending another parameter, like so:

```sh
bower_components/angular-template-bundler/build-template-bundle.js tmpl templates.json
```

If you have your templates written to a file other than `templates.json`, you need to tell the runtime code about it, like so:

```js
myApp.config(['templateBundlerConfigProvider', function (templateBundlerConfigProvider) {
  templateBundlerConfigProvider.setBundleURL('tmpl.json')
}])
```




## How it works

`templateBundler` hooks into angular's `$templateRequest` service, and after the templates needed for the initial view (the first one after the user arrives on the page) have been downloaded, it will answer all subsequent requests for templates with its own `promise`s while downloading the templates bundle file. When the download of said file is completed, the contained templates become available to the whole app and all the promises are resolved in the normal way.

With this in place, there are three phases to template requests:

1. The initial request for the first view – this should be as fast, and therefore as small, as possible
2. Template loading in aggregate – all templates are now being downloaded in one single request
3. All templates reside in $templateCache and are conveniently served from there through the default pathway

### $templateRequest
This works on the `$templateRequest` service. So if you use a custom template strategy that eshews that service,
this will not work as a drop-in solution.

### View the Code, Luke
For more in-depth knowledge, **peruse the code** – it's quite simple really, and extensively documented =)