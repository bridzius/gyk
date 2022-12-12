# [Grow Your Knowledge] Advanced Routing

## Feature modules

Dokumentacija: https://angular.io/guide/feature-modules

### Aprašymas

Angular aplikacijai augant, `.module` failas gali tapti perkrautas įvairiais skirtingais komponentais ir funkcionalumais. Tokiu atveju, yra įprasta elementus suskirstyti pagal funkcionalumą į 'Feature' modulius. Tokie moduliai įprastai egzistuoja šalia pagrindinio `app.module.ts` ir yra skirti būtent skirtingo funkcionalumo komponentų grupavimui.

Feature moduliai sukuriami tiesiog sugeneruojant naują modulį - `npx ng generate module <vardas>` ir perkeliant komponentus, kurie jam yra skirti. Tuo pačiu, reikia nepamiršti importuoti modulių į savo `app.module.ts` failo `imports`.

## Lazy loading

Dokumentacija: https://angular.io/guide/lazy-loading-ngmodules

### Aprašymas

Įprastai, Angular routinimas veikia "eager loading" principu. "Eager loading" reiškia, kad tuo metu, kai vartotojas bando nueiti į puslapį, puslapis jau būna atsiųstas ir pakrautas. Mažoms aplikacijoms tai yra labai patogu, kadangi visas kodas būna parsiunčiamas iš karto ir vartotojui nereikia papildomai laukti nuėjus į puslapį. Tuo tarpu dideli puslapiai gali užimti ir dešimt ar daugiau megabaitų, taigi siųsti tiek informacijos užtrunka papildomai laiko. Aplikacijoms, padalintoms į [Feature Modulius](#feature-modules), Angular leidžia pritaikyti Lazy Loading strategiją.

Angular Lazy Loading leidžia krauti tam tikro puslapio kodą tik tada, kai užeinama į to puslapio URL, arba suvedus naršyklėje, arba naudojant `Router`. Taip yra sumažintas pradinio kodo kiekis, o puslapių kodas yra kraunamas tik tada, kai tie puslapiai reikalingi. Lazy Loading yra labai naudinga strategija, norint padidinti pradinio puslapio krovimo greitį, arba turint puslapių, kur vartotojai užeina retai.

### Naudojimas

Lazy loading reikalauja Feature Module, kadangi (standartiškai) toks krovimas reikalauja atskiros route konfiguracijos lazy loaded dalyje. "Įjungti" Lazy loading galima pakeitus `component` savo route konfiguracijoje į `loadChildren`, ir naudojant Javascript dinaminį `import()`:

```ts
  { path: "news", component: NewsPageComponent },
```

į

```ts
  { path: "news", loadChildren: import("./news/news.module.ts").then(m => m.NewsModule) },
```

Svarbu nepamiršti, kad, lazy loading atveju, visa routing konfiguracija bus perkelta į modulį, kurį kraunate. Taigi pavyzdyje parodytu `/news` atveju, visi sekantys URL - `/news/*` turi būti aprašyti `NewsModule` viduje per `RouterModule.forChild`. Pirminis - "lazy loaded" - route turės būti tuščias bei atitiks `/news` URL:

```ts
//news.module.ts
@NgModule({
    //...
    imports: [
        //...
        RouterModule.forChild([{
            path: "", component: NewsComponent // URL: /news
        }, {
            path: ":item", component: NewsItemComponent // URL: /news/{item}
        }])
    ]
})
```

## Guards

Dokumentacija: https://angular.io/guide/router#preventing-unauthorized-access

### Aprašymas

Angular Guards yra specialios paskirties servisai, skirti valdyti navigaciją. Generuojami su `npx ng generate guard <name>`. Guard'e galima apibrėžti kelis metodus, kurie įgyvendina skirtingą logiką. Logika paremta `true/false` grąžinimu iš metodo:

- `canActivate` - leidžia vartotojui patekti į URL.
- `canActivateChild` - leidžia vartotojui patekti į šio Route "vaikus" (route, kurie eina po šio).
- `canDeactivate` - pasako, ar gali vartotojas išeiti iš URL.
- `canLoad` - leidžia vartotojui pakrauti "lazy loaded" modulį.
- `canMatch` - pasako, ar galima leisti vartotoją į route, ar reikia ieškoti kito route.

### Naudojimas

Guard'ai yra naudojami aprašant juos savo `Route` konfiguracijoje:

```ts
@NgModule({
    //...
    imports: [
        //...
        RouterModule.forRoot([
            {
                path: "news",
                component: NewsComponent,
                canActivate: [CanActivateGuard],
                canActivateChild: [CanActivateChildGuard],
                canDeactivate: [CanDeactivateGuard],
                canLoad: [CanLoadGuard],
                canMatch: [CanMatchGuard]
            }
        ])
    ]
})
```

## Užduotis

https://github.com/bridzius/gyk-routing-advanced


