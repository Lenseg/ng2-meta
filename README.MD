# ng2-ui-router-meta

Update HTML meta tags for title, description and others automatically based on the state in your Angular2 app width ui-router.

This is an Angular2 SEO/meta tags module. For the Angular 1.x module, check out [ngMeta](https://github.com/vinaygopinath/ngMeta)

## Getting started

### Install
To install this library, run:

```bash
$ npm install ng2-ui-router-meta --save
```

ng2-ui-router-meta works with angular 4.0.0 and ui-router 1.0.0 or higher.

### Modify views

Add meta tags to states. By default, `title` and `description` values are duplicated for `og:title` and `og:description`, so there's no need to add them separately.
<pre>
<code>
const forgotPassState = {
  parent: 'app',
  name: 'forgot-pass',
  url: '/forgot-pass',
  data:{
    meta:{
      title:'Password reset',
      description:'Reset password via email.'
    }
  },
  component: ForgotPassComponent
}
</code>
</pre>

### Import MetaModule

<pre>
<code>
...
<strong>import { MetaModule } from 'ng2-ui-router-meta';</strong>

@NgModule({
  declarations: [
    AppComponent,
    ...
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule,
    RouterModule.forRoot(routes),
    <strong>MetaModule.forRoot()</strong>
  ],
  bootstrap: [AppComponent]
})
</code>
</pre>

### Update AppComponent

<pre>
<code>
<strong>import { MetaService } from 'ng2-ui-router-meta';</strong>

@Component({
  ...
})
export class AppComponent {
  constructor(<strong>private metaService: MetaService</strong>) {}
}
</code>
</pre>

You're all set! ng2-ui-router-meta will update the meta tags whenever the route changes.

## Options

### Set defaults

Set default values for tags. These values are used when routes without `meta: {}` information are encountered.
```typescript
import { MetaConfig, MetaService } from 'ng2-ui-router-meta';

const metaConfig: MetaConfig = {
  //Append a title suffix such as a site name to all titles
  //Defaults to false
  useTitleSuffix: true,
  defaults: {
    title: 'Default title for pages without meta in their route',
    titleSuffix: ' | Site Name',
    'og:image': 'http://example.com/default-image.png',
    'any other': 'arbitrary tag can be used'
  }
};

@NgModule({
  declarations: [
    AppComponent,
    ...
  ],
  imports: [
    ...,
    MetaModule.forRoot(metaConfig)
  ],
  bootstrap: [AppComponent]
})

```

### Change meta tags programmatically
```typescript
import { Component, OnInit } from '@angular/core';

class ProductComponent implements OnInit {
  ...
  constructor(private metaService: MetaService) {}
  
  ngOnInit() {
    this.product = //HTTP GET for product in catalogue
    this.metaService.setTitle('Product page for '+this.product.name);
    this.metaService.setTag('og:image',this.product.imageURL);
  }
}
```

### Define fallback meta content in HTML
While Google executes Javascript and extracts meta tags set by ng2-meta, many bots (like Facebook and Twitter) do not execute Javascript. Consider defining fallback meta tags in your HTML for such bots. The fallback content is overridden by ng2-meta in Javascript-enabled environments.

```html
<html>
  <head>
    <meta name="title" content="Website Name">
    <meta name="og:title" content="Website Name">
    <meta name="description" content="General description of your site">
    <meta name="og:description" content="General description of your site">
    <meta name="og:image" content="http://example.com/fallback-image.png">
  </head>
</html>
```

## Development

To generate all `*.js`, `*.js.map` and `*.d.ts` files:

```bash
$ npm run build
```

To lint all `*.ts` files:

```bash
$ npm run lint
```
