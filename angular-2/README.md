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

### Servisai

Generuojami `npx ng generate service <vardas>`, Angular servisai yra paprastos Typescript klasės, su `@Injectable` anotacija, kuriose galima rašyti logiką ir ta logika dalintis tarp kitų Angular elementų.

## Observables - https://angular.io/guide/observables

## HttpClient - https://angular.io/guide/http

## Routing - https://angular.io/guide/routing-overview

## Namų darbai

```

```
