# [Grow Your Knowledge] - Angular antra dalis

## Directives - https://angular.io/guide/built-in-directives

Angular elementai, kurie prideda papildomo funkcionalumo jau egzistuojantiems komponentams.
Pažymima `@Directive` anotacija.

```ts
@Directive({
    selector: '[appDirektyva]' //CSS selectorius
})
```

Standartiškai skirstomos į tris tipus:

1. Komponentai - komponentai irgi yra direktyvos, kurios turi HTML template.
2. Atributų direktyvos - pakeičia tam tikro elemento veikimą arba išvaizdą.
3. Struktūrinės direktyvos - pakeičia elementų struktūrą bei HTML.

### Atributų direktyvos

Standartinės atributų direktyvos:

- `ngStyle` - prideda `style` atributą elementui ir leidžia aprašyti stilius.
- `ngClass` - prideda naują CSS klasę, priklausomai nuo parametro reikšmės.
- `ngModel`

### Struktūrinės direktyvos

Standartinės struktūrinės direktyvos:

- `*ngIf` - paslepia elementą, priklausomai nuo komponento parametro reikšmės.
- `*ngFor` - iteruoja per masyvą (array) ir kiekvienam masyvo elementui sukuria po HTML elementą.
- `ngSwitch` - parodo tinkamą HTML template pagal parametro reikšmę.

Išsiskiria `*` priekyje. Tai reiškia, kad kompiliuojant kodą HTML bus pakeistas, pridedant papildomus elementus:

```ts
<span *ngIf="isTrue"></span>
```

Tampa

```ts
<ng-template [ngIf]="isTrue">
    <span></span>
</ng-template>
```

```ts
<span *ngFor="let skaicius of [1,2,3]"> {{skaicius}} </span>
```

Tampa

```ts
<ng-template ngFor let-skaicius [ngForOf]="[1,2,3]">
    <span> {{skaicius}} </span>
</ng-template>

```

**SVARBU** - Negalima naudoti dviejų struktūrinių direktyvų ant vieno HTML elemento.

### Pavyzdys:

Pagal https://github.com/bridzius/gyk-ng-intro

1. Sukuriame Tėvo komponente vaikų masyvą

```ts
//tevas.component.ts
vaikai = ["Petras", "Ona", "Leonardokadijus", "Skaivelina"];
```

2. Iteruojame per vaikus ir priskiriame vaiko reikšmę kaip vardą

```html
<!-- tevas.component.html -->
<app-vaikas
  (paspaudimas)="keisti()"
  vardas="{{ vaikas }}"
  *ngFor="let vaikas of vaikai"
></app-vaikas>
```

3. Sukuriame komponente parametrą, pagal kurį paslėpsime vaikus bei funkciją, kuri keis parametro reikšmę

```ts
//tevas.component.ts
rodytiVaikus = true;
sleptiVaikus() {
    this.rodytiVaikus = !this.rodytiVaikus;
}
```

4. Sukuriam mygtuką, kuris slepia/rodo vaikus. Naudojame Angular specialų `ng-container` elementą, kad galėtume uždėti `*ngIf `visiem vaikų komponentams

```html
<!-- tevas.component.html -->
<ng-container *ngIf="rodytiVaikus">
  <app-vaikas
    (paspaudimas)="keisti()"
    vardas="{{ vaikas }}"
    *ngFor="let vaikas of vaikai"
  >
  </app-vaikas>
</ng-container>
<button (click)="sleptiVaikus()">Paslepti vaikus</button>
```

### Rezultatas - https://github.com/bridzius/gyk-ng-intro/tree/directives #directives branch.

## Dependency Injection (DI) ir Servisai https://angular.io/guide/dependency-injection-overview

Dependency Injection yra principas, kuris leidžia Angular elementams (komponentams/direktyvoms/servisams) naudoti servisus ir kitus elementus savyje. Tai leidžia lengvai pernaudoti funkcionalumą ir turėti paprastus komponentus, o logiką - servisuose.

Angular naudoja "type-based" Dependency Injection per konstruktorius. Tai reiškia, kad norint pasinaudoti DI, reikia naudoti savo norimą dependency kaip tipą konstruktoriuje, kur norėsite jį naudoti. Pvz, norint panaudoti ` KazkoksService``TevasComponent `viduje:

```ts
//tevas.component.ts
import { Component } from "@angular/core";
import { KazkoksService } from "kazkoks.service";
@Component({
  //...
})
class TevasComponent {
  constructor(private service: KazkoksService) {
    this.service.metodas(); //galime naudoti metodus iš KazkoksService.
  }
}
```

### Servisai

Generuojami `npx ng generate service <vardas>`, Angular servisai yra paprastos Typescript klasės, su `@Injectable` anotacija, kuriose galima rašyti logiką ir ta logika dalintis tarp kitų Angular elementų.

### Dependency Injection principai

Angular "supranta" tris lygius dependency injection:

1. Aplikacijos lygio (Singleton)
2. Modulio lygio
3. Komponento lygio

### 1. Aplikacijos lygio DI

Standartinis DI, generuotuose servisuose. Šis DI metodas tinkamas 99% atvejų ir tik retai kada reikia kitų dviejų. Aprašomas:

```ts
//kazkoks.service.ts
import { Injectable } from "@angular/core";
@Injectable({ providedIn: "root" }); //Aplikacijos lygio DI
class KazkoksService {}
```

Sukuria vieną servisą visam aplikacijos kontekstui. Tai reiškia, kad tas pats serviso bus naudojamas visuose komponentuose, kurie jį importuos. Taip pat, kad jo reikšmės yra matomos visų komponentų.

### 2. Modulio lygio DI

Servisą galima "pririšti" prie tam tikro modulio, jei norima, kad servisas būtų sukurtas tik to modulio lygmenyje ir būtų reikalinga importuoti modulį, norint naudoti servisą. Taip galima tą patį servisą aprašyti keliuose skirtinguose moduliuose, jei norima šiek tiek pakeisti jo veikimą. Aprašomas pačiame servise:

```ts
//kazkoks.service.ts
import { Injectable } from "@angular/core";
import { KazkoksModule } from "./kazkoks.module";
@Injectable({ providedIn: KazkoksModule }); //Modulio lygio DI
class KazkoksService {}
```

arba modulio `providers` dalyje:

```ts
//kazkoks.module.ts
import { NgModule } from "@angular/core";
import { KazkoksService } from "./kazkoks.service";
@NgModule({
  //...
  providers: [KazkoksService],
})
class KazkoksModule {}
```

### 3. Komponento lygio DI

Servisą taip pat galima sukurti atskirai kiekvienam komponentui, kur jis reikalingas. Tai patogu, jei komponentai daug kartų naudojami ir pakankamai komplikuoti - tarkim, ilgos formos - ir reikalauja daug logikos, kurią galima pernaudoti, bet nereikia dalintis. Aprašomi komponento anotacijoje:

```ts
//tevas.component.ts
import { Component } from "@angular/core";
import { KazkoksService } from "./kazkoks.service";
@Component({
  //...
  providers: [KazkoksService],
})
class TevasComponent {}
```

### Pavyzdys

Perkelkime vardus, naudojamus `TevasComponent` į `VardaiService`. Tai leis komponentui nesirūpinti iš kur vardai ateina ir galėsime veliau juos gauti kad ir iš serverio.

1. Sugeneruojame servisą - `npx ng generate service vardai`
2. Perkeliame vardus iš komponento į servisą (komponente paliekame tuščią masyvą) ir sukuriame metodą juos gauti

```ts
//vardai.service.ts
export class VardaiService {
  private vaikai = ["Petras", "Ona", "Leonardokadijus", "Skaivelina"];

  gaukVardus() {
    return this.vaikai;
  }
}
```

3. Naudojame servisą `TevasComponent` viduje ir gauname vardus

```ts
//tevas.component.ts
import { VardaiService } from "../vardai.service";
//...
export class TevasComponent {
    rodytiVaikus = true;
    vaikai: string[] = [];
    constructor(private vardaiService: VardaiService) {
        this.vaikai = this.vardaiService.gaukVardus();
    }
//...
```

### Rezultatas - https://github.com/bridzius/gyk-ng-intro/tree/service #service branch.

## Observables - https://angular.io/guide/observables

TODO: Observables

## HttpClient - https://angular.io/guide/http

TODO: HttpClient with Meteo

## Routing - https://angular.io/guide/routing-overview

## Namų darbai
