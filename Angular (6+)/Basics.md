# Angular (6+)

### Modules

- basic building blocks of Angular app
- provide context for components & group related code into functional sets
- all apps have at least a *root module* for bootstrapping & starting the app (like a main method) and typically many more feature modules.


### Architecture
- services and components are just classes w/ decorator attributes to set that apart from each other. 
- each component has a *template* attribute that defines it's html template.
- components shouldn't fetch data from server, validate user input, etc -> they delegate these tasks to services so component code is lean and efficient.
- each service has an @injectable decorator to tell Angular what it can be injected into via DI.


![Image of Angular Organization](https://angular.io/generated/images/guide/architecture/overview2.png)


### Organization
- organizing into distinct functional modules helps: 
    
    1) managing development of complex apps
    2) designing for reusability
    3) take advantage of lazy loading (modules only loaded when needed - less at startup which makes app faster)

### Module - Metadata

- *declarations* - components, directives, & pipes that __belong__ to the module.
- *exports* - subset of declarations that should be visible and usable in components of other modules (think of like an access modifier)
- *imports* - other modules whose exported classses are needed by components in __this__ module.
- *providers* - creators of services that this module contriudes to the global collection of services so it's available in all parts of your app.
- *bootstrap* - main app view (called root component) which hosts all other app views. This should only be set in root module.

```
import { NgModule }      from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
@NgModule({

  //needed modules for this module
  imports:      [ BrowserModule ], 

  //services exposed to app level in this module
  providers:    [ Logger ], 

  //components belonging to this module
  declarations: [ AppComponent ], 

  //subset of declarations that are exported
  exports:      [ AppComponent ], 

  //"main" - starting component for entire app
  bootstrap:    [ AppComponent ] 

})
export class AppModule { }
```

### ngModule vs JS Modules
- Angular modules (ngModule) are different from JS's (ES2015) module system. These are complementary systems that you can use together to write your apps.
- In JS, each file is a module that you can export, then import into another file.

### Angular Libraries 
- Angular loads as a collection of JS modules (think of them as library modules).
- each is prefixed w/ @angular and can be imported as needed into your app.



TODO: https://angular.io/guide/architecture-components