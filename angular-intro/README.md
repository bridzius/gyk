# [Grow Your Knowledge] - Angular Intro

## Components

Komponentai - pagrindiniai Angular "statybiniai" elementai. Angular - "component based" framework.

Generuojama `npx ng generate component <vardas>`

## Directives

Angular komponentai, kurie prideda papildomo funkcionalumo jau egzistuojantiems komponentams. Standartiškai skirstomos į tris tipus:

1. Komponentai - komponentai irgi yra direktyvos, kurios turi HTML template.
2. Atributų direktyvos - pakeičia tam tikro elemento veikimą arba išvaizdą.
3. Struktūrinės direktyvos - pakeičia elementų struktūrą bei HTML.

### Atributų direktyvos

Standartinės atributų direktyvos:

- `ngStyle`
- `ngClass`
- `ngModel`

### Struktūrinės direktyvos

Standartinės struktūrinės direktyvos:

- `*ngIf`
- `*ngFor`
- `ngSwitch`

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

Generuojama `npx ng generate directive <vardas>`

## DI

## Services
