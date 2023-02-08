# Angular Internationalization (i18n)


Application example built with [Angular](https://angular.io/) 15 and adding the internationalization (i18n) component using the [@ngx-translate/core](https://www.npmjs.com/package/@ngx-translate/core) library.

This tutorial was posted on my [blog](https://rodrigo.kamada.com.br/blog/adicionando-o-componente-de-internacionalizacao-i18n-em-uma-aplicacao-angular) in portuguese and on the [DEV Community](https://dev.to/rodrigokamada/adding-the-internationalization-i18n-component-to-an-angular-application-4pac) in english.



[![Website](https://shields.braskam.com/v1/shields?name=website&format=rectangle&size=small&radius=5)](https://rodrigo.kamada.com.br)
[![LinkedIn](https://shields.braskam.com/v1/shields?name=linkedin&format=rectangle&size=small&radius=5)](https://www.linkedin.com/in/rodrigokamada)
[![Twitter](https://shields.braskam.com/v1/shields?name=twitter&format=rectangle&size=small&radius=5&socialAccount=rodrigokamada)](https://twitter.com/rodrigokamada)
[![Instagram](https://shields.braskam.com/v1/shields?name=instagram&format=rectangle&size=small&radius=5)](https://www.instagram.com/rodrigokamada)



## Prerequisites


Before you start, you need to install and configure the tools:

* [git](https://git-scm.com/)
* [Node.js and npm](https://nodejs.org/)
* [Angular CLI](https://angular.io/cli)
* IDE (e.g. [Visual Studio Code](https://code.visualstudio.com/))



## Getting started


### Create the Angular application


**1.** Let's create the application with the Angular base structure using the `@angular/cli` with the route file and the SCSS style format.

```shell
ng new angular-internationalization
? Would you like to add Angular routing? Yes
? Which stylesheet format would you like to use? SCSS   [ https://sass-lang.com/documentation/syntax#scss                ]
CREATE angular-internationalization/README.md (1073 bytes)
CREATE angular-internationalization/.editorconfig (274 bytes)
CREATE angular-internationalization/.gitignore (604 bytes)
CREATE angular-internationalization/angular.json (3339 bytes)
CREATE angular-internationalization/package.json (1090 bytes)
CREATE angular-internationalization/tsconfig.json (783 bytes)
CREATE angular-internationalization/.browserslistrc (703 bytes)
CREATE angular-internationalization/karma.conf.js (1445 bytes)
CREATE angular-internationalization/tsconfig.app.json (287 bytes)
CREATE angular-internationalization/tsconfig.spec.json (333 bytes)
CREATE angular-internationalization/src/favicon.ico (948 bytes)
CREATE angular-internationalization/src/index.html (313 bytes)
CREATE angular-internationalization/src/main.ts (372 bytes)
CREATE angular-internationalization/src/polyfills.ts (2820 bytes)
CREATE angular-internationalization/src/styles.scss (80 bytes)
CREATE angular-internationalization/src/test.ts (788 bytes)
CREATE angular-internationalization/src/assets/.gitkeep (0 bytes)
CREATE angular-internationalization/src/environments/environment.prod.ts (51 bytes)
CREATE angular-internationalization/src/environments/environment.ts (658 bytes)
CREATE angular-internationalization/src/app/app-routing.module.ts (245 bytes)
CREATE angular-internationalization/src/app/app.module.ts (393 bytes)
CREATE angular-internationalization/src/app/app.component.scss (0 bytes)
CREATE angular-internationalization/src/app/app.component.html (24617 bytes)
CREATE angular-internationalization/src/app/app.component.spec.ts (1139 bytes)
CREATE angular-internationalization/src/app/app.component.ts (233 bytes)
✔ Packages installed successfully.
```

**2.** Install and configure the Bootstrap CSS framework. Do steps 2 and 3 of the post *[Adding the Bootstrap CSS framework to an Angular application](https://github.com/rodrigokamada/angular-bootstrap)*.

**3.** Install the `@ngx-translate/core` and `@ngx-translate/http-loader` libraries.

```shell
npm install @ngx-translate/core @ngx-translate/http-loader
```

**4.** Import the `HttpClient`, `HttpClientModule`, `TranslateModule`, `TranslateLoader` and `TranslateHttpLoader` modules. Change the `app.module.ts` file and add the lines as below.

```typescript
import { HttpClient, HttpClientModule } from '@angular/common/http';
import { TranslateModule, TranslateLoader } from '@ngx-translate/core';
import { TranslateHttpLoader } from '@ngx-translate/http-loader';

export function createTranslateLoader(http: HttpClient) {
  return new TranslateHttpLoader(http, './assets/i18n/', '.json');
}

imports: [
  BrowserModule,
  HttpClientModule,
  TranslateModule.forRoot({
    loader: {
      provide: TranslateLoader,
      useFactory: (createTranslateLoader),
      deps: [HttpClient],
    },
    defaultLanguage: 'en-US',
  }),
  AppRoutingModule,
],
```

**5.** Create the `de-DE.json`, `en-US.json`, `es-ES.json`, `fr-FR.json` and `pt-BR.json` files in the `src/assets/i18n` folder.

**6.** Add the content below in the `de-DE.json` file.

```json
{
  "hello": "Hallo, {{name}}!"
}
```

**7.** Add the content below in the `en-US.json` file.

```json
{
  "hello": "Hello, {{name}}!"
}
```

**8.** Add the content below in the `es-ES.json` file.

```json
{
  "hello": "Hola, {{name}}!"
}
```

**9.** Add the content below in the `fr-FR.json` file.

```json
{
  "hello": "Bonjour, {{name}}!"
}
```

**10.** Add the content below in the `pt-BR.json` file.

```json
{
  "hello": "Olá, {{name}}!"
}
```

**11.** Remove the contents of the `AppComponent` class from the `src/app/app.component.ts` file. Import the `TranslateService` service and create the `changeLanguage` method as below.

```typescript
import { Component } from '@angular/core';
import { TranslateService } from '@ngx-translate/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss'],
})
export class AppComponent {

  constructor(public translateService: TranslateService) {
  }

  public changeLanguage(language: string): void {
    this.translateService.use(language);
  }

}
```

**12.** Remove the contents of the `src/app/app.component.html` file. Add the buttons as below.

```html
<div class="container-fluid py-3">
  <h1>Angular Internationalization (i18n)</h1>

  <div class="btn-group btn-group-sm py-5">
    <button type="button" class="btn btn-sm btn-outline-secondary" (click)="changeLanguage('de-DE')" [class.active]="translateService.currentLang === 'de-DE'">Deutschland</button>
    <button type="button" class="btn btn-sm btn-outline-secondary" (click)="changeLanguage('en-US')" [class.active]="!translateService.currentLang || translateService.currentLang === 'en-US'">English</button>
    <button type="button" class="btn btn-sm btn-outline-secondary" (click)="changeLanguage('es-ES')" [class.active]="translateService.currentLang === 'es-ES'">Español</button>
    <button type="button" class="btn btn-sm btn-outline-secondary" (click)="changeLanguage('fr-FR')" [class.active]="translateService.currentLang === 'fr-FR'">Francés</button>
    <button type="button" class="btn btn-sm btn-outline-secondary" (click)="changeLanguage('pt-BR')" [class.active]="translateService.currentLang === 'pt-BR'">Português</button>
  </div>

  <h3>{{ "hello" | translate: { name: "Angular" } }}</h3>
</div>
```

**13.** Run the application with the command below.

```shell
npm start

> angular-internationalization@1.0.0 start
> ng serve

✔ Browser application bundle generation complete.

Initial Chunk Files | Names         |      Size
vendor.js           | vendor        |   2.57 MB
styles.css          | styles        | 266.58 kB
polyfills.js        | polyfills     | 128.54 kB
scripts.js          | scripts       |  76.67 kB
main.js             | main          |  13.03 kB
runtime.js          | runtime       |   6.66 kB

                    | Initial Total |   3.05 MB

Build at: 2021-08-15T20:12:53.818Z - Hash: 9462fdcfd1de35681ab4 - Time: 11933ms

** Angular Live Development Server is listening on localhost:4200, open your browser on http://localhost:4200/ **


✔ Compiled successfully.
```

**14.** Ready! Access the URL `http://localhost:4200/` and check if the application is working. See the application working on [GitHub Pages](https://rodrigokamada.github.io/angular-internationalization/) and [Stackblitz](https://stackblitz.com/edit/angular15-internationalization).

![Angular Internationalization](https://res.cloudinary.com/rodrigokamada/image/upload/v1639156375/Blog/angular-internationalization/angular-internationalization.png)



## Cloning the application

**1.** Clone the repository.

```shell
git clone git@github.com:rodrigokamada/angular-internationalization.git
```

**2.** Install the dependencies.

```shell
npm ci
```

**3.** Run the application.

```shell
npm start
```
