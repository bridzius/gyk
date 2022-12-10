# [Grow Your Knowledge] Advanced Routing

## Feature modules

Dokumentacija: https://angular.io/guide/feature-modules

Angular aplikacijai augant, `.module` failas gali tapti perkrautas įvairiais skirtingais komponentais ir funkcionalumais. Tokiu atveju, yra įprasta elementus suskirstyti pagal funkcionalumą į 'Feature' modulius. Tokie moduliai įprastai egzistuoja šalia pagrindinio `app.module.ts` ir yra skirti būtent skirtingo funkcionalumo komponentų grupavimui.

Feature moduliai sukuriami tiesiog sugeneruojant naują modulį - `npx ng generate module <vardas>` ir perkeliant komponentus, kurie jam yra skirti. Tuo pačiu, reikia nepamiršti importuoti modulių į savo `app.module.ts` failo `imports`.

## Lazy loading

Dokumentacija: https://angular.io/guide/lazy-loading-ngmodules

## Guards

Dokumentacija: https://angular.io/guide/router#preventing-unauthorized-access
