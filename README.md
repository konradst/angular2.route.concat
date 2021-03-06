# angular2.route.concat
Concat multiple @angular/router RouterConfigs so you can keep component route definitions inside component folders

## Example

1. Create some example component with a route
  ```
  // ./app/user/user.routes.ts
  import {ConcatRoute} from "../route/index";
  import {RouterConfig} from '@angular/router';
  import {UserHomeComponent} from './index';
  
  // if it were the whole application, this would be your final RouterConfig
  const config:RouterConfig = [
    {path: 'user-home', component: UserHomeComponent}
  ];
  
  // but we'll create a ConcatRoute object (to give it order) and export it
  // we want user routes be resolved first
  export const routes:ConcatRoute = {
    order: 1,
    routerConfig: config
  };
  ```
2. Merge array of routes of multiple components into one  by applying concatRoute
  ```
  // ./app/app.routes.js
  import {provideRouter} from '@angular/router';
  import {concatRoutes} from './route/index';
  import {routes as userRoutes} from './user/user.routes';
  import {routes as pageRoutes} from './page/page.routes';
  
  //the magic takes place here (create single RouteConfig from 2 other ones)
  export const appRouterProviders = [
    provideRouter(concatRoutes([userRoutes, pageRoutes]))
  ];
  ```
3. Bootstrap your app with your router providers (just like you would do after reading @angular/router tutorial)
  ```
  // ./main.js
  import {appRouterProviders} from './app/app.routes';
  
  bootstrap(AppComponent, [
    appRouterProviders
  ])
    .catch(err => console.error(err));
  ```
  
## Todo

package.json
