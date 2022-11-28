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

### Pavyzdys

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

### Aprašymas

Norėdami supaprastinti Angular programavimą bei sujungti sinchroninio (veiksmai vyksta vienas po kito) bei asinchroninio (veiksmų seka nelinijinė) programavimo principus, kūrėjai nusprendė daugeliai Angular konceptų naudoti `Observables`. Tai yra nestandartinis Javascript objektas, kilęs iš "Reactive" (nemaišyti su React) programavimo, ir galintis parodyti daug reikšmių per laiką. `Observable` galima įsivaizduoti kaip vamzdį, kuris priima reikšmes iš šaltinio (source) ir perduoda klausytojams (listeners). Priimti galima ir vieną ir daug reikšmių, bet jos bus perduotos visiems klausytojams.

### Naudojimas

Angular `Observables` naudojami iš `rxjs` bibliotekos (https://rxjs.dev/) - `import { Observable } from "rxjs";` Dauguma Angular servisų turi galimybę gražinti Observable, tačiau jį galima sukurti ir patiems. Observable kuriame per `Observable` konstruktorių ir jam paduodame funkciją, kurioje yra reikšmės, atiduodamos klausytojams:

```ts
import { Observable } from "rxjs";
const obs = new Observable((sub) => {
  sub.next(1); //klausytojai (listeners) gaus reikšmę '1'
});
```

Observable klausymas prasideda naudojant `.subscribe()` metodą. _**SVARBU**: Dažniausiai Observables pradeda veikti tik, kai kas nors jų klausosi._ Pamirštas `subscribe` - viena iš dažniausių su `Observable` sutinkamų problemų. Jei norėtume gauti reikšmes iš mūsų prieš tai sukurto observable `obs` - tereikia subscribe'inti:

```ts
obs.subscribe((val) => console.log(val)); //Gautume '1'
```

### Operatoriai

Taip pat Observables leidžia keisti reikšmę prieš ją perduodant klausytojams naudojant `pipe()` metodą. `pipe()` priima funkcijas, vadinamas "operatoriais", kurios leidžia modifikuoti observable reikšmę. Pavyzdžiui galėtume pakeisti `obs` reikšmę su `map` operatoriumi taip:

```ts
import { Observable } from "rxjs";
import { map } from "rxjs/operators";
const obs = new Observable((sub) => {
  sub.next(1); //klausytojai (listeners) gaus reikšmę '1'
});
obs.pipe(map((val) => `Numeris ${val}`)).subscribe((val) => console.log(val)); //Gautume 'Numeris 1'
```

Operatorius galima naudoti ne vien reikšmėms keisti, bet ir observable kūrimui arba kelių observable kombinavimui - daugiau apie juos skaitykite https://rxjs.dev/guide/operators#categories-of-operators

### AsyncPipe

Naudojant daug observablų, kodas gali greitai užsiteršti bereikalingais `subscribe()` kvietimais. Taip pat būtina nepamiršti, kad dažniausiai reikia kviesti `unsubscribe()`, kai observable reikšmių mums nebereikia (pavyzdžiui klientas nuėjo į kitą puslapį ir jam to komponento nebereikia). Tokiems atvejams palengvinti - Angular turi `AsyncPipe`. `AsyncPipe` yra Angular Pipe (https://angular.io/guide/pipes), t.y HTML naudojamas pagalbinis metodas. Jo padedami, galime pačiame HTML išgauti observable reikšmes, nenaudodami `subscribe/unsubscribe`. Tai sumažina kodo, o taip pat ir klaidų, kiekį.

`obs` naudokime kaip komponento parametrą.

```ts
//example.component.ts
import { Component } from "@angular/core";
import { Observable } from "rxjs";
@Component({
  //...
})
class ExampleComponent {
  obs = new Observable((sub) => {
    sub.next(1); //klausytojai (listeners) gaus reikšmę '1'
  });
}
```

Pridėkime `async` (AsyncPipe) jo rodymui HTML. Pastebėkite, kad niekur nenaudojame `subscribe()`:

```html
<!-- example.component.html -->
{{obs | async}}
<!-- rodys 1 -->
```

## HttpClient - https://angular.io/guide/http

### Aprašymas

Dažniausiai komunikacijos tarp serverio ir kliento (web aplikacijos) būdas - HTTP kvietimai. HTTP protokolas leidžia pakankamai paprastai keistis informacija. Angular turi standartinį servisą, skirtą HTTP komunikacijai - `HttpClient`. Jis leidžia naudoti standartinius HTTP metodus dirbti su išoriniais API. Kadangi komunikacija asinchroninė - visi `HttpClient` naudojami metodai (`get/post/put/delete/patch/t.t`) grąžina `Observable`, kuris perduos iš serverio atkeliavusią informaciją.

### Pavyzdys

Pakeiskime savo `VardaiService` vardus vardais, gaunamais iš API (https://vardai.fdp.workers.dev). **SVARBU**: Įsitikinkite, kad jūsų puslapis pasiekiamas per `http://localhost:4200`:

1. Pridedame `HttpClientModule` prie savo Angular Module. Tai mum leis naudoti `HttpClient`:

```ts
//app.module.ts
import { HttpClientModule } from "@angular/common/http";
@NgModule({
//...
  imports: [BrowserModule, AppRoutingModule, HttpClientModule],
})
//...
```

2. Importuojame `HttpClient` savo servise:

```ts
//vardai.service.ts
import { HttpClient } from "@angular/common/http";
//...
export class VardaiService {
  constructor(private http: HttpClient) {}
  //...
}
```

3. Naudojame GET HTTP metodą vardams gauti `gaukVardus` funkcijoje:

```ts
export class VardaiService {
  constructor(private http: HttpClient) {}

  gaukVardus(): Observable<any> {
    return this.http.get("https://vardai.fdp.workers.dev");
  }
}
```

4. Pakeičiame `this.vaikai` reikšme typescript kode, kadangi tai dabar observable:

```ts
//tevas.component.ts
export class TevasComponent {
  //...
  vaikai: Observable<any>; // Pakeitėme iš string[]
  constructor(private vardaiService: VardaiService) {
    this.vaikai = this.vardaiService.gaukVardus();
  }
  //...
}
```

5. Naudojame `AsyncPipe` HTML'e, kad gautume reikšmes. Pipe galime kombinuoti su bet kuria reikšme. Šiuo atveju su iteratoriumi `*ngFor`:

```html
<!-- tevas.component.html -->
<app-vaikas
  (paspaudimas)="keisti()"
  vardas="{{ vaikas }}"
  *ngFor="let vaikas of vaikai | async"
>
</app-vaikas>
```

6. HTML matome '[object Object] yra vaikas.'. Atsidarę 'Network Inspector' naršyklėje pamatysite, kad API gražina kitokią struktūra, nei mes prieš tai naudojome komponente. Mes tikimės paprastų `string`, o API gražina `{ "name": string }`. Panaudokime `map()``operatorių  
   servise gauti šias reikšmes:

```ts
//...
import { map } from "rxjs/operators";
export class VardaiService {
  constructor(private http: HttpClient) {}

  gaukVardus(): Observable<any> {
    return (
      this.http
        //paduodame generic, kad Typescript žinotų, kokio atsakymo tikimės
        .get<{ name: string }[]>("https://vardai.fdp.workers.dev")
        //Naudojame map() observable reikšmei
        .pipe(
          map((zmones) => {
            //Naudojant Array.map() iš žmonių masyvo gauname tik vardų masyvą
            return zmones.map((zmogus) => zmogus.name);
          })
        )
    );
  }
}
```

### Rezultatas - https://github.com/bridzius/gyk-ng-intro/tree/http #http branch.

## Routing - https://angular.io/guide/routing-overview

## Namų darbai
