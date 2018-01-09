# single-spa-examples
https://single-spa.surge.sh

This is an example of examples on how to use [single-spa](https://github.com/joeldenning/single-spa) in a variety of contexts. If you're looking for a simpler example that's just a webpack starter for single-spa, try out [simple-single-spa-webpack-example](https://github.com/joeldenning/simple-single-spa-webpack-example). Right now there are examples with the following technologies:

- React
- AngularJS
- Angular
- react-router
- angular-ui-router
- Webpack
- Babel
- Vue
- Svelte
- Ember
- Inferno
- Preact

## View the demo!
A [demo is live](http://single-spa.surge.sh) on surge.sh.

## This project is an Angular 5 portal as microfrontend lazy loaded

## How to get the examples running locally
```bash
git clone git@github.com:PlaceMe-SAS/single-spa-examples.git
cd single-spa-examples
npm install
npm run start
open http://localhost:8080
```

### Serve your angular project
```bash
npm install -g @angular/cli
cd src/home
npm install
ng serve
open http://localhost:4200
```

### For production apps mode
```bash
ng build --prod
```
And replace the target url of your child app

repeat for all angular cli projects

## Add an angular cli apps
```bash
cd src
ng new app1 --prefix=app1
cd app1
ng serve --port=4201
```

```
// src/app1/loader.js

import singleSpaAngularMicroFrontend from '../util/single-spa-angular-mf';

const lifecycles = singleSpaAngularMicroFrontend({
    selector: 'app1-root',
    baseScriptUrl: 'http://localhost:4201',
    scripts: [
        'inline.bundle.js',
        'polyfills.bundle.js',
        'styles.bundle.js',
        'vendor.bundle.js',
        'main.bundle.js'
    ]
});

export const bootstrap = [
    lifecycles.bootstrap
];

export const mount = [
    lifecycles.mount
];

export const unmount = [
    lifecycles.unmount
];
```

```
// src/app1/src/polyfills.ts

// Comment zone.js, it is globaly imported by the portal
// import 'zone.js/dist/zone';  // Included with Angular CLI.
```

```
// src/app1/src/index.html

  <app1-root></app1-root>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/zone.js/0.8.19/zone.js"></script>
</body>
```

```
// src/main.js

declareChildApplication('app1', () => import('./app1/loader.js'), hashPrefix('/app1'));
```

```
// src/app1/src/main.ts

import { enableProdMode } from '@angular/core';
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';

import { singleSpaAngularPlatform } from '../../util/single-spa-angular-platform';

import { AppModule } from './app/app.module';
import { environment } from './environments/environment';

declare const window;

if (environment.production) {
  enableProdMode();
}

singleSpaAngularPlatform.mount('app1-root', (unmount) => {
  platformBrowserDynamic().bootstrapModule(AppModule).then(unmount);
});
```
