# [Grow Your Knowledge] - Angular Intro

## Angular naujo workspace struktūra

- `.angular/` - cache direktorija, kurią galima ignoruoti.
- `.vscode/` - nustatymai VSCode editoriui
- `node_modules/` - npm dependency direktorija, kurioje saugomos visos bibliotekos, kurias naudoja jūsų projektas.
- `dist/` - direktorija, kurioje randamas į serverį paruoštas dėti serveris. Sukuriama `ng build` komandos.
- `src` - kodo ir implentacijos direktorija, kurioje yra darbiniai failai.
  - `app` - Angular aplikacijos kodas
    - `app-routing.module.ts` - Angular routing konfigūracija, skirta naviguoti tarp skirtingų puslapių.
    - `app.component.html` - Angular komponento (`app-root`) template failas.
    - `app.component.scss` - Angular komponento (`app-root`) stilių failas.
    - `app.component.spec.ts` - Angular komponento (`app-root`) testų failas.
    - `app.component.ts` - Angular komponento (`app-root`) klasės failas.
    - `app.module.ts` - Angular modulis, kuris aprašo komponentus, randamus aplikacijoje.
  - `assets/` - statiniai failai, naudojami aplikacijoje. Pvz nuotraukos.
  - `favicon.ico - favicon failas (rodomas browser tab'e).
  - `index.html` - pagrindinis index.html failas, kuriame generuojama Angular aplikacija.
  - `main.ts` - kodas Angular kodo užkrovimui.
  - `styles.scss` - globalių stilių failas.
- `.editorconfig` - editoriaus nustatymai, tokie kaip kabučių tipas ir kt.
- `.gitignore` - git'o ignoruotini failai.
- `angular.json` - Angular projekto konfigūracijos failas.
- `package-lock.json` - npm dependencies sąrašas, pagal kurį bus instaliuojama.
- `package.json` - npm projekto konfigūracijos failas su visais dependencies.
- `README.md` - projekto aprašymas.
- `tsconfig.app.json` - typescript compiler konfigūracija Angular aplikacijai.
- `tsconfig.json` - bendroji typescript compiler konfigūracija.
- `tsconfig.spec.json` - typescript compiler konfigūracija testams.

## `angular.json` struktūra

`angular.json` randami pagrindiniai projekto nustatymai:

```json
...
"architect": {
    // nustatymai skirtingoms Angular komandoms:
    // ng build nustatymai
    "build": {
        "builder": "@angular-devkit/build-angular:browser",
        "options": {},
        "configurations": {
            //ng build --configuration=production
            "production": {},
            //ng build --configuration=development
            "development": {}
        },
        "defaultConfiguration": "production"
    },
    //ng serve
    "serve": {
        "builder": "@angular-devkit/build-angular:dev-server",
        "configurations": {
        //ng serve --configuration=production
        "production": {
            "browserTarget": "angular-intro:build:production"
        }
    }
}
...
```

## Components - https://angular.io/guide/component-overview

Komponentai - pagrindiniai Angular "statybiniai" elementai. Angular - "component based" framework. Jie veikia tėvo-vaiko (parent-child) principu ir gali dalintis informacija per parametrus (tėvas > vaikui) bei event'us (vaikas > tėvui).

Generuojama `npx ng generate component <vardas>`

```ts
@Component({
    selector: 'app-component', //Komponento Selektorius, CSS formatu
    templateUrl: './app.component.html', //Komponento HTML template
    styleUrls: ['./app.component.css'], //CSS failas su stiliais
})
```

Angular komponentas priima parametrus per `@Input` bei gali grąžinti eventus per `@Output EventEmitter`

Pavyzdys:

```html
<!-- Struktūra: -->
<app-root>
  <app-tevas>
    <app-vaikas></app-vaikas>
  </app-tevas>
</app-root>
```

```html
<!-- tevas.component.html -->
<app-vaikas (paspaudimas)="keisti()" vardas="Petras"></app-vaikas>
```

```html
<!-- vaikas.component.html -->
<p (click)="log()">{{vardas}} yra vaikas.</p>
```

```ts
// tevas.component.ts;
export class TevasComponent {
  keisti() {
    console.log("Pakeista");
  }
}
```

```ts
// vaikas.component.ts;
export class VaikasComponent {
  @Input() vardas: string = "";
  @Output() paspaudimas: EventEmitter<object> = new EventEmitter();
  log() {
    console.log("Paspaudė");
    this.paspaudimas.emit();
  }
}
```

HTML bus rodoma:
`Petras yra vaikas`

Konsolėje paspaudus ant `Petras yra vaikas` matysite:

```bash
Paspaudė
Pakeista
```

### Rezultatas - https://github.com/bridzius/gyk-ng-intro
