# [Grow Your Knowledge] HTML/CSS/JavaScript

Pagrindai.

## HTML paskaita

HTML elementai ir jų aprašymai - https://www.w3schools.com/tags/default.asp

## CSS Paskaita: Pakopinės Stiliaus Paklodės

- CSS pradmenys - https://web.dev/learn/css/
- CSS advanced - http://css-tricks.com
- CSS selector žaidimas - https://flukeout.github.io/
- CSS grid žaidimas - https://cssgridgarden.com/
- CSS flexbox žaidimas - https://flexboxfroggy.com/
- CSS paskaitos pavyzdys - https://stackblitz.com/edit/gyk-css?file=index.html (atidatyti per Chrome/Edge)

### Selectoriai

```
h1 { /* selectinami 'h1' tag'ai */
  background: red;
}
```

### Taisyklės

```
h1 {
  background: red;
  font: italic 500 20px 'Courier New', Courier, monospace;
  /* shorthand - tas pats, kas apačioje */
  font-family: 'Courier New', Courier, monospace;
  font-weight: 500;
  font-size: 20px;
  font-style: italic;
}
```

### Prioritetai

Svarbu: Child (>) arba sibling (.klasė.klasė) stiliai turi prioritetą prieš standartinius.
Pavyzdžiui `main > h1` bus labiau prioritizuotas, nei `h1`. Taip pat ir `h1.klasė`, jei `h1` turi klasę.

1. Stilius su `!important`.
2. Stilius `style` atribute ant elemento.
3. Stilius pagal #id.
4. Stilius pagal .klasę.
5. Stilius pagal <tagą>.
6. Standartiniai naršyklės stiliai.
