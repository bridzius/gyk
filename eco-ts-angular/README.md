# [Grow Your Knowledge] NodeJS/Typescript/Angular

## Typescript

### Aprašymas

- Įrašomas su `npm` -> `npm install typescript`.
- Typescript yra Javascript Superset. Visas Javascript kodas yra validus Typescript kodas. Typescript = Javascript + tipai.
- Typescript kompiliuojamas* į Javascript. (*iš tikrųjų transpiliuojamas, bet istoriškai įrankis vadinamas "Typescript compiler");
- Typescript naudojamas tik programos rašymo metu. Kompiliuojant visi tipai yra "išimami" ir kodas pakeičiamas paprastu Javascript.
- Standartiškai aprašomas _.ts failuose. Pakeitus _.js failą į \*.ts gaunamas validus Typescript failas.

### Tipai

- Primitive - tipai atitinkantys pagrindinius Javascript tipus:
  - `string` - 'string'
  - `number` - 22
  - `boolean` - true
- Abstract - abstraktūs tipai, apibrėžiantys papildomas Javascript reikšmes:
  - `any` - "bet koks" tipas, leidžiantis apeiti Typescript tipų tikrinimą.
  - `unknown` - šiuo metu nežinomas tipas. Naudojant šį tipą, reikės jį castinti (žr [Castinimas](#castinimas).
  - `object` - bendrinis objekto tipas.
  - `null` - aprašo `null` objektą. Tai yra reikšmė, kuri specifiškai nurodyta kaip neegistuojanti.
  - `undefined` - kintamojo reikšmė nėra priskirta.
  - `void` - tipas, reiškiantis, kad kintamasis neegzistuoja arba niekas nėra gražinama iš funkcijos.
- Enum - tipai, aprašantys specifines reikšmes, kurios gali būti naudojamos. Naudojamas `enum`.
- Type - custom tipai. Naudojamas `type`.
- Interface - objektus arba klases aprašantys tipai. Naudojamas `interface`.
- Union - tipas, leidžiantis rinktis vieną iš kelių tipų. Rašomas per `|`, pvz `let value: string | number;` leidžia `value` būti arba `string`, arba `number`.

Dažniausiai aprašomi per dvitaškius:

```ts
const name: string = "Vardas";
```

### Generic tipai

Galima sukurti tipus, kurie priima kitus tipus kaip kintamuosiuos, taip leidžiant pakeisti jų galutinę reikšmę. Tai leidžia turėti standartizuotus tipus, kurių reikšmes galima keisti priklausomai nuo situacijos. Generic tipai aprašomi su `<T>`.

```ts
enum Automobilis { //enum su automobilio tipais
    Lengvasis,
    Komercinis,
}

type Zmogus<T> { // T yra generic
    vardas: string;
    pavarde: string;
    transportas: T //automobilio tipas bus T
}

// Tipas priimamas kaip Zmogus su Automobiliu.
const asmuo: Zmogus<Automobilis> = {
    vardas: "vardas",
    pavarde: "pavarde",
    transportas: Automobilis.Lengvasis
}
```

Daugiau info apie generic tipus: https://www.typescriptlang.org/docs/handbook/2/generics.html#hello-world-of-generics

### Pagalbiniai tipai

Pagalbiniai (utility) tipai naudojami kartu su kitais tipais jų pakeitimui:

- `Omit<Type, 'parameter'>` - Sukuria nauja objekto tipą tik su visais kito tipo parametrais išskyrus pažymėtus.
- `Pick<Type, 'parameter'>`- Sukuria nauja objekto tipą tik su pasirinktais kito tipo parametrais.
- `Partial<Type>` - Visus objekto tipo/interface parametrus padaro "Optional" - `{param?: value}`.
- `Required<Type>` - Visus objekto tipo/interface parametrus padaro "Required".

  Daugiau info apie utility tipus: https://www.typescriptlang.org/docs/handbook/utility-types.html

### Castinimas

Typescript leidžia "keisti" tipus į kitus naudojant castinimą. Taip galima susiaurinti tipus arba aiškiau apibrėžti juos kviečiant funkcijas arba naudojant. Castinimas veikia naudojant `as`raktažodį.

```ts
let value: any;
value = 1;

callNumber(value as number);

function callNumber(number: number): void {
  call(number);
}
```

### Pavyzdžiai

```ts
// Tipų priskyrimas
let vardas: string = "vardas";
let amzius: number = 30;
------
// Galima sukurti tipus iš kitų tipų
type Vardas = string;
let vardas: Vardas = "vardas";
------
//Objekto tipą galima aprašyti arba type, arba interface
enum Transportas {
    Automobilis,
    Troleibusas,
    Batai
}

type Zmogus = {
  vardas: string;
  pavarde: string;
  transportas: Transportas
};

interface Zmogus {
  vardas: string;
  pavarde: string;
  transportas?: Transportas //Optional tipai aprašomi su klaustuku.
};
------
//Utility tipai leidžia keisti objekto parametrus ir jų prieinamumą
type Zmogus = {
    vardas: string,
    pavarde?: string
};

type OptionalZmogus = Partial<Zmogus>;
/* tas pats kaip
type OptionalZmogus = {
    vardas?: string, //Vardas nebuvo optional
    pavarde?: string
}
*/

type RequiredZmogus = Required<Zmogus>;
/* tas pats kaip
type RequiredZmogus = {
    vardas: string,
    pavarde: string //Pavarde anksciau buvo optional
}
*/

type ZmogausVardas = Pick<Zmogus, 'vardas'>;
/* tas pats kaip
type ZmogausVardas = {
    vardas: string //Pasirinktas tik vardas
}
*/
----
//Klasė gauna tipą ir gali būti naudojama kaip tipas.
class Zmogus {
    vardas: string;
    pavarde: string;
};

const zmogus: Zmogus = new Zmogus();
zmogus.vardas = "vardas";
zmogus.pavarde = "pavarde";

const kitasZmogus: Zmogus = {
    vardas: "vardas",
    pavarde: "pavarde",
}
----
//Funkcijos gali turėti ir input ir return tipus
function run(input: string /* input tipas */): string /* return tipas */ {
    return input;
}
----
//Generic tipai leidžia keisti objekto parametrus
//Veikia su type ir su funkcijomis
type Zmogus<Gyvunas> = {
    vardas: string;
    pavarde: string;
    gyvunas: Gyvunas;
}

enum GyvunoRusis {
    Kate,
    Suo
}

type Augintinis<Rusis> = {
    vardas: string;
    rusis: Rusis;
};

type Kate = Augintinis<GyvunoRusis.Kate>;
type Suo = Augintinis<GyvunoRusis.Suo>;
type ZmogusSuSunim = Zmogus<Suo>;
type ZmogusSuKate = Zmogus<Kate>;

//Sukurs masyvą (array) su 5 gyvūno tipo elementais
function duokPenkisGyvunus<Gyvunas>(gyvunas: Gyvunas): Gyvunas[] {
  return Array(5).fill(gyvunas);
}

const gyvunas: Suo = {
  vardas: 'Amsius',
  rusis: GyvunoRusis.Suo,
};

const penkiSunys: Suo[] = duokPenkisGyvunus<Suo>(gyvunas);
```
