# [Grow Your Knowledge] Angular Forms

## HTML Formos

Populiaru sakyti, kad internetiniai puslapiai būna dviejų tipų: sąrašai/lentelės ir formos. Lenteles naudojame duomenų atvaizdavimui, o formas - jų įvedimui.
HTML forma aprašoma `<form>` elementu. Dažniausiai forma yra sukurta iš vieno arba kelių `<input>` elementų ir `<button>`, kuris išsiunčia formos duomenis - kitaip sakant "submit'ina". Dokumentacijoje dažnai rasite "Form Control" pavadinimą - taip yra vadinamas vienas `<input>`, kuris laiko vieną formos reikšmę.

Forma gali atrodyti taip:

```html
<form>
  <!-- Vardo input, kurį reikia pateikti -->
  <input name="vardas" required type="text" placeholder="Įrašykite vardą" />
  <!-- Amžiaus input, kurio nebūtina įrašyti  -->
  <input name="amzius" min="18" type="number" placeholder="Įrašykite amžių" />
  <button type="submit">Išsiųsti</button>
</form>
```

Duomenų gavimas iš formos Javascript'e yra šiek tiek labiau kompleksinis. Javascript pagalba turėsite rasti `<form>` elementą ir iš jo galėsite gauti `vardas` ir `amzius` (pagal `name` atributą) reišmes.

### HTML Validatoriai

Intro: https://developer.mozilla.org/en-US/docs/Learn/Forms/Form_validation

HTML validatoriai yra atributai, kurie yra naudojami

## Formos

Angular leidžia mums lengviau dirbti su duomenų įvedimu, bei palaiko dviejų tipų formas:

1. 'Template-driven' formas.
2. 'Reactive' formas. (Typed - Standartas nuo Angular 14 | Untyped - Standartas iki Angular 14)

### Template driven formos

Dokumentacija: https://angular.io/guide/forms

Template driven formos dažnai vadinamos "pagerintomis HTML formomis". Šios formos vis dar naudoja visus standartinius HTML atributus ir standartinius HTML validatorius, bet Angular perima reikšmių valdymą.
Jos vadinamos "Template Driven", nes visa forma yra aprašome template (HTML) ir tik duomenų modelis aprašomas Typescript.

Naudojama pridedant prie modulio `FormsModule`.

```ts
import { FormsModule } from "@angular/forms";
//...
@NgModule({
//...
  imports: [FormsModule],
})
```

#### Duomenų modelis

Template-driven formos kūrimas prasideda sukuriant duomenų modelį, kuris yra atspindimas formoje. Jei naudosime pirmiau minėtą HTML formą kaip pavyzdį, mūsų duomenų modelis gali atrodyti kaip Typescript klasė:

```ts
//asmuo.ts
export class Asmuo {
  constructor(public vardas: string, public amzius?: number) {}
}
```

Tada sukursime standartinį modelį formos komponente.

```ts
//form.component.ts
import { Asmuo } from "asmuo";
//...
export class FormComponent {
  model: Asmuo = new Asmuo(""); //nurodome pirminę modelio reikšmę.

  onSubmit() {
    console.log(this.model); //Atspausdiname dabartinę modelio reikšmę į konsolę kai submittiname.
  }
}
```

#### ngModel

`ngModel` yra naudojamas kaip "klijai", kurie suriša HTML formą ir duomenų modelį, sukurtą Angular komponente. Jis leidžia komponente matyti formos reikšmes ir tuo pačiu template matyti reikšmių pakeitimus iš komponento. `ngModel` dažniausiai aprašomas two-way-data-binding sintakse, kaip `[(ngModel)]`, kadangi jis ir perduoda reikšmes iš template į komponentą, ir klausosi pakitimų iš komponento, kad galėtų juos atvaizduoti template.

HTML forma, naudojant `ngModel` ir prieš tai sukurtą modelį atrodytų taip:

```html
<!-- Komponente forma vadinsis 'asmuoForm' -->
<form #asmuoForm="ngForm" (ngSubmit)="onSubmit()">
  <!-- Dėmesio: "name" atributas ir modelio parametro pavadinimas turi sutapti -->
  <input
    name="vardas"
    [(ngModel)]="model.vardas"
    required
    type="text"
    placeholder="Įrašykite vardą"
  />
  <input
    name="amzius"
    [(ngModel)]="model.amzius"
    min="18"
    type="number"
    placeholder="Įrašykite amžių"
  />
  <button type="submit">Išsiųsti</button>
</form>
```

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

## Pavyzdys

https://github.com/IndirasM/gyk-routing/tree/forms
