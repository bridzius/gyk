# [Grow Your Knowledge] Angular Forms

## HTML Formos
Populiaru sakyti, kad internetiniai puslapiai būna dviejų tipų: sąrašai/lentelės ir formos. Lenteles naudojame duomenų atvaizdavimui, o formas - jų įvedimui.
HTML forma aprašoma `<form>` elementu.

## Formos

Angular palaiko trijų tipų formas:
1. 'Template-driven' formas.
2. 'Untyped Reactive' formas. (Standartas iki Angular 14)
3. 'Typed Reactive' formas. (Standartas nuo Angular 14)

### Template driven formos
Naudojama pridedant prie modulio `FormsModule`.
```ts
import { FormsModule } from "@angular/forms";
//...
@NgModule({
//...
  imports: [FormsModule],
})
```

### Untyped Reactive formos
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
### Typed Reactive formos
Iki Angular 14, Reactive formos buvo "Untyped". Pati forma neturėjo apibrėžto duomenų modelio, taigi tikrinant bei validuojant duomenis reikėjo rašyti papildomą kodą, kuris "užtikrintų", kad duomenys egzistuoja.
