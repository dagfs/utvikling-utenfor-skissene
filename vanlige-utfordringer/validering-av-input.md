<style>
    label{
        display:block;
    }

    .label{
        display:inline-block;
        min-width:100px;
    }

    input:invalid {
        outline: 1px solid red;
    }

    input+.error {
        display: none;
    }

    input:invalid+.error {
        color: red;
        display: inline;
    }

    input+.timed-error {
        display: none;
    }

    input.timed-error {
        outline: none !important;
    }

    input.timed-error:invalid {
        outline: 1px solid red;
    }

    input[showError="true"]:invalid+.timed-error {
        color: red;
        display: inline;
    }
</style>

# Validering av input

Det er hovedsaklig to strategier for valigering av input. Validering i backend, og validering i frontend. Backend
validering må gjøres uansett, og vil kunne gi brukeren gode tilbakemeldinger, men vil kreve at brukeren lagrer data
før de får se om noe er feil. Validering i frontend gir brukeren tilbakemelding med en gang og gir ofte en bedre
brukeropplevelse hvis det blir gjort på en god måte. Det er også mulig å kombinere ved å sende inn dataene brukeren
skriver inn underveis for å returnere eventuelle feil.

Videre i denne artikkelen tar vi utgangspunkt i at den beste fremgangsmåten er å gjøre validering av input i frontend også, og ser derfor på hvordan dette gjøres på en god måte, både teknisk og for brukeropplevelsen. Til slutt runder vi av med et løsningsforslag.

## Hvordan validere

[HTML5 inneholder nå mange funksjoner for å validere input](https://developer.mozilla.org/en-US/docs/Learn/Forms/Form_validation#Using_built-in_form_validation). Det har blitt så bra at en i stor grad kan løse validering med disse. Hvis en kombinerer denne valideringen med [input typer](https://developer.mozilla.org/en-US/docs/Learn/Forms/HTML5_input_types) kan en skape en god brukeropplevse med minimal bruk av JavaScript.

De innebygde egenskapene(attributt) som brukes for validering er:

- `required`
- `type`
- `minlength` og `maxlength` - lengde av strenger
- `min` og `max` - verdier av nummer
- `pattern`

Ved å sette type og valideringsregler vil inputfeltet selv ofte kunne si ifra om det er gyldig eller ikke. Dette gjøres ved at pseudo-class (tenk `:hover`) `invalid` blir satt både på `form` og `input` elementet det gjelder.

Vi kan derfor enkelt style et inputfelt som ikke har gyldige verdier på denne måten:

```css
input:invalid {
  outline: 1px solid red;
}
```

### Egenskapen `required`

Den første egenskapen vi skal se på et `required`. `required` støttes av de fleste input typene, med untak av `color` og `range` som har default verdier. Elementer med egenskapen `required` får også pseudo-class `:required`, og feltene som ikke har den satt får tilsvarende pseudo-class `optional`. `reuired` gjør det en forventer. Den gjør at et felt som mangler verdi blir satt til ugyldig.

#### Eksempel

```html
<label> Påkrevd felt: <input required /> </label>
```

<label>
    Påkrevd felt: <input required>
</label>

[Les mer om `required` i MDN we docs](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/required)

### Egenskapen `type`

Egenskapen `type` påvirker hvordan et input felt fungerer. Default input type er `text`. Noen av verdiene vil vise egne komponenter / utvidede inputfelter som begrenser brukerens muligheter til verdier de kan skrive inn.

Mulige verdier for egenskapen `type` er:

- `button` - viser en knapp. Foreslår å heller bruke tagen `button`.
- `checkbox`
- `color` - lar brukeren velge farge via en fargevelger eller ved å skrive inn fargen på formatet `#rrggbb`.
- `date` - lar en bruker skrive inn en dato på formatet `yyyy-mm-dd`
- `datetime-local` - lar en bruker skrive inn en dato på formatet `yyyy-mm-ddThh:mm`
- `email` - NB! godkjenner epostaddresser uten TLD (feks. dag.frode@example)
- `file` - kan definere lovlige formater ved `accept="image/png, image/jpeg"`.
- `hidden`
- `image` - tilsvarer `submit`, men en kan velge et bilde som skal vises.
- `month` - lar en bruker skrive inn en månede på formatet `yyyy-mm`
- `number`
- `password`
- `radio`
- `range`
- `reset` - resetter alle verdier i formet. Ikke anbefalt å bruke da det muligjør brukerfeil.
- `search` - inputfelt med mulighet for å tømme feltet
- `submit` - lagre knapp i formet
- `tel` - type for å skrive inn telefonnummere. Valideres ikke men hjelper brukeren på mobil
- `text`
- `time` - lar en bruker skrive inn en månede på formatet `hh:mm`
- `url`
- `week` - lar en bruker skrive inn en månede på formatet `yyyy-W##`

#### De uten validering

Typene `text`, `search`, `hidden`, `password`, `tel` kommer uten noen form for validering. En skulle kanskje tro `password` var begrenset til `[a-zA-Z0-9_\-.]`, men det altså faktisk ikke det! `tel` blir ofte gruppert med `url` og `email`, men telefonnummere har forskjellige formater og kan derfor ikke løses på noen god måte i et slikt felt.

<label>
<span class="label">Text: </span>
<input type="text">
</label>

<label>
<span class="label">Tel: </span>
<input type="tel">
</label>

<label>
<span class="label">Search: </span>
<input type="search">
</label>

<label>
<span class="label">Password: </span>
<input type="password">
</label>

#### De med mønstre

<label>
<span class="label">Email: </span>
<input type="email">
</label>

<label>
<span class="label">URL: </span>
<input type="url">
</label>

#### De med velgere

<label>
<span class="label">Range: </span>
<input type="range">
</label>

<label>
<span class="label">Color: </span>
<input type="color">
</label>

<label>
<span class="label">File: </span>
<input type="file">
</label>

#### De med mønstre og velgere

<label>
<span class="label">Number: </span>
<input type="number">
</label>

<label>
<span class="label">Time: </span>
<input type="time">
</label>

<label>
<span class="label">Week: </span>
<input type="week">
</label>

<label>
<span class="label">Date: </span>
<input type="date">
</label>

<label>
<span class="label">Datetime-local: </span>
<input type="datetime-local">
</label>

<label>
<span class="label">Month: </span>
<input type="month">
</label>

#### De som i seg selv ikke kan være feil

<label>
<span class="label">Radio: </span>
<input type="radio">
</label>

<label>
<span class="label">Checkbox: </span>
<input type="checkbox">
</label>

#### De som ikke har så mye å validere

<label>
<span class="label">Button: </span>
<input type="button" value="Button">
</label>

<label>
<span class="label">Submit: </span>
<input type="submit">
</label>

<label>
<span class="label">Reset: </span>
<input type="reset">
</label>

<label>
<span class="label">Image: </span>
<input type="image">
</label>

[Les mer om `type` i MDN we docs](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input#%3Cinput%3E_types)

### Egenskapene `minLength` og `maxLength`

Egenskapene `minLength` og `maxLength` sier hvor mange tegn et bruker skal få skrive inn i et `input` eller `textarea`. `maxLength` forhinder brukeren fra å skrive inn mer enn max antall tegn, mens `minLength` vil gjøre feltet ugyldig fra det første tegnet er skrevet inn, til `minLength` er nådd.

#### Eksempel

```html
<label> Inputfelt for 3-5 tegn: <input minlength="3" maxlength="5" /> </label>
```

<label>
    Inputfelt for 3-5 tegn: <input minLength="3" maxLength="5">
</label>

[Les mer om `minLength` i MDN we docs](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/minlength)

[Les mer om `maxLength` i MDN we docs](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/maxLength)

### Egenskapene `min` og `max`

Egenskapene `min` og `max` sier noe om den minste og største godkjente verdien til et `input`. Nettleseren vil prøve å forhindre brukeren fra å skrive inn ugyldige verdier og sette `input` til `:invalid` hvis verdien går utenfor `min` og `max`.
Egenskapene kan brukes på `input` av følgende typer:

- `date` (yyyy-mm-dd)
- `month` (yyyy-mm)
- `week` (yyyy-W##)
- `time` (hh:mm)
- `datetime-local` (yyyy-mm-ddThh:mm)
- `number`
- `range`

#### Eksempel - Inputfelt med verdi mellom 3 og 5

```html
<label>
  Inputfelt med verdi mellom 3 og 5: <input type="number" min="3" max="5" />
</label>
```

<label>
    Inputfelt med verdi mellom 3 og 5: <input type="number" min="3" max="5">
</label>

#### Eksempel - Inputfelt med dato mellom 1 januar 2020 og 1 april 2020

```html
<label>
  Inputfelt med dato mellom 1 januar 2020 og 1 april 2020:
  <input type="date" min="2020-01-01" max="2020-04-01" />
</label>
```

<label>
Inputfelt med dato mellom 1 januar 2020 og 1 april 2020: <input type="date" min="2020-01-01" max="2020-04-01">
</label>

[Les mer om `min` i MDN we docs](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/min).

[Les mer om `max` i MDN we docs](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/max)

## Brukeropplevelse

https://medium.com/@andrew.burton/form-validation-best-practices-8e3bec7d0549

For å skape en god brukeropplevelse ved input validering er det viktig å tenke på:

### 1. Hvor en viser feilene

### 2. Vise feilmeldingene på riktig tidspunkt

### 3. Bruke fornuftige farger

### 4. Bruk tydelig språk

<label>
    Epost: <input type="email">
    <span class="error">Venligst skriv inn en gyldig epost</span>
</label>
<br>
<br>
<form>
    <label>
        Epost: <input class="timed-error" type="email" required showError="false"
            oninput="this.setAttribute('showError', false)" onblur="this.setAttribute('showError', true)">
        <span class="timed-error">Vennligst skriv inn en gyldig epostadresse</span>
    </label>
    <button>Lagre</button>
</form>

---

## setCustomValidity

<label>
    <input type="number" oninvalid="setCustomValidity('Venligst fyll inn et tall mellom 5 og 20000')"
        oninput="setCustomValidity('')" min="5" max="20000" value="1">
    <span></span>
</label>
https://developer.mozilla.org/en-US/docs/Web/API/HTMLSelectElement
