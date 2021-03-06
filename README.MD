[![Build Status](https://travis-ci.org/mpalourdio/ng-http-loader.svg?branch=master)](https://travis-ci.org/mpalourdio/ng-http-loader)  

# ng-http-loader

## Changelog

[Please read the changelog](CHANGELOG.MD)

## Installation

To install this library, run:

```bash
$ npm install ng-http-loader --save
```

## What does it do ?

This package provides an HTTP Interceptor, and a spinner component (All from [SpinKit](https://github.com/tobiasahlin/SpinKit) at the moment).
The HTTP interceptor listens to all HTTP requests and shows a spinner during pending requests.

## Angular 4 / Angular 5

The latest compatible module version with angular 4 is **``0.3.4``**.
If you want to use Angular 5, use **``^0.4.0``**.

If you experience errors like below, **please double check the version you use.**

``ERROR in Error: Metadata version mismatch for module [...]/angular/node_modules/ng-http-loader/ng-http-loader
  .module.d.ts, found version 4, expected 3, resolving symbol AppModule in [...]/angular/src/app.module.ts``
  

## Requirements

Compatible with Angular 4.3+. Performing http requests with the new API from ``HttpClientModule`` is **mandatory**. Otherwise,
the spinner will not be fired **at all**.

## HttpClientModule

All http requests **MUST** be performed with the new ``HttpClientModule`` available from ``angular 4.3``. See this [blog post](http://blog.ninja-squad.com/2017/07/17/http-client-module/) as reference.

## Usage

From your Angular `AppModule`:

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
[...]
import { AppComponent } from './app.component';
import { HttpClientModule } from '@angular/common/http'; <============
import { NgHttpLoaderModule } from 'ng-http-loader/ng-http-loader.module'; <============

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    HttpClientModule, <============ (Perform http requests with this module)
    NgHttpLoaderModule, <============
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

or (splitted modules mode for more convenience)

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
[...]
import { AppComponent } from './app.component';
import { HttpClientModule } from '@angular/common/http'; <============
import { NgHttpLoaderComponentsModule } from 'ng-http-loader/components/ng-http-loader-components.module'; <============
import { NgHttpLoaderServicesModule } from 'ng-http-loader/services/ng-http-loader-services.module'; <============

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    HttpClientModule, <============ (Perform http requests with this module)
    NgHttpLoaderServicesModule, <============
    NgHttpLoaderComponentsModule, <============
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

In your app.component.html, add :
```xml
<spinner></spinner>
```

## Customizing the spinner

You can customize the background-color and the spinner type:
```xml
<spinner [backgroundColor]="'#ff0000'" [spinner]="spinkit.skWave"></spinner>
```

**_To use this syntax, you must reference the ``Spinkit`` const as a public property in you app.component.ts_**:

```typescript
import { Spinkit } from 'ng-http-loader/spinkits'; <============

@Component({
    selector: 'app-root',
    templateUrl: './app.component.html',
    styleUrls: ['./app.component.css'],
})
export class AppComponent {
    public spinkit = Spinkit; <============
    [...]
}
```
The different spinners available are referenced in [this class](src/spinkits.ts).


**_Otherwise, you can simply reference the chosen spinner as a simple string_**:

```xml
<spinner backgroundColor="#ff0000" spinner="sk-wave"></spinner>
```

## Requests filtering

You can filter the requests that shouldn't be caught by the interceptor by providing **an array of regex patterns**:
```xml
<spinner 
    [backgroundColor]="'#ff0000'"
    [spinner]="spinkit.skWave"
    [filteredUrlPatterns]="['\\d', '[a-zA-Z]', 'my-api']">
</spinner>
```

## Misc

Each Spinkit component defined in [SPINKIT_COMPONENTS](src/spinkits.ts#L30) can be used independently.

## Credits

The spinners/loaders have been taken from [SpinKit](https://github.com/tobiasahlin/SpinKit).
