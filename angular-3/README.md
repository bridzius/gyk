# [Grow Your Knowledge] Angular Forms

## HTML Formos

Populiaru sakyti, kad internetiniai puslapiai būna dviejų tipų: sąrašai/lentelės ir formos. Lenteles naudojame duomenų atvaizdavimui, o formas - jų įvedimui.
HTML forma aprašoma `<form>` elementu.

## Formos

Angular palaiko dviejų tipų formas:

1. 'Template-driven' formas.
2. 'Reactive' formas. (Typed - Standartas nuo Angular 14 | Untyped - Standartas iki Angular 14)

### Template driven formos

Dokumentacija: https://angular.io/guide/forms

Naudojama pridedant prie modulio `FormsModule`.

```ts
import { FormsModule } from "@angular/forms";
//...
@NgModule({
//...
  imports: [FormsModule],
})
```

#### ngModel

#### HTML Validatoriai

### Reactive formos -

Dokumentacija: https://angular.io/guide/reactive-forms

Template-driven formos yra labai lanksčios, tačiau jų prisirišimas prie HTML template reiškia, kad Typescript komponento kode sunku suprasti, kas vyksta.
Reactive formos sprendžia šią problemą, leisdamos visą formą, kartu su validatoriais ir duomenų modeliu sukurti Typescript kode ir HTML tik "prijungti" elementus prie formos modelio.

Naudojama pridedant prie modulio `ReactiveFormsModule`.

```ts
import { ReactiveFormsModule } from "@angular/forms";
//...
@NgModule({
//...
  imports: [ReactiveFormsModule],
})
```

#### FormBuilder

Dokumentacija: https://angular.io/guide/reactive-forms#using-the-formbuilder-service-to-generate-controls

#### Validators

Dokumentacija: https://angular.io/guide/form-validation#validating-input-in-reactive-forms
