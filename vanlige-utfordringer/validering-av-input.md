<style>
    label {
        display: block;
    }

    .label {
        display: inline-block;
        min-width: 100px;
    }

    input:invalid {
        outline: 1px solid red;
    }

    .error,
    .error-next-line {
        display: none;
    }

    input:invalid+.error {
        margin-left: 10px;
        color: red;
        display: inline;
    }

    input:invalid+.error-next-line {
        color: red;
        display: block;
        margin-top: 4px;
        margin-left: 100px;
    }

    .timed-error {
        display: none;
    }

    input[show-error="false"]:invalid {
        outline: none;
    }

    input[show-error="true"]:invalid {
        outline: 1px solid red;
    }

    input[show-error="true"]:invalid+.timed-error {
        color: red;
        display: inline;
    }
</style>

# Validering av input

Det er hovedsaklig to strategier for valigering av input. Validering i backend, og validering i frontend. Backend
validering må gjøres uansett, og vil kunne gi brukeren gode tilbakemeldinger, men vil kreve at brukeren sender inn data
før de får se om noe er feil. Validering i frontend gir brukeren tilbakemelding med en gang og gir ofte en bedre
brukeropplevelse hvis det blir gjort på en god måte. Det er også mulig å kombinere ved å sende inn dataene brukeren
skriver inn underveis for å returnere eventuelle feil.

Videre i denne artikkelen tar vi utgangspunkt i at den beste fremgangsmåten er å gjøre validering av input i frontend
også, og ser derfor på hvordan dette gjøres på en god måte, både teknisk og for brukeropplevelsen. Til slutt runder vi
av med et løsningsforslag.

> Hvis en har mulighet til å bruke samme valideringen i frontend og backend, er dette å anbefale. Feks hvis en bruker JS
> i frontend og backend og deler objekter og validering mellom prosjektene. Fordelen dette bringer med seg er at
> valideringen er helt lik, noe som ofte ikke er tilfellet hvis en gjør valideringen uavhangig i to forskjellige språk.

## Hvordan validere

[HTML5 inneholder nå mange funksjoner for å validere
input](https://developer.mozilla.org/en-US/docs/Learn/Forms/Form_validation#Using_built-in_form_validation). Det har
blitt så bra at en i stor grad kan løse validering med disse. Hvis en kombinerer denne valideringen med [input
typer](https://developer.mozilla.org/en-US/docs/Learn/Forms/HTML5_input_types) kan en skape en god brukeropplevse med
minimal bruk av JavaScript.

De innebygde egenskapene(attributt) som brukes for validering er:

- `required`
- `type`
- `minlength` og `maxlength` - lengde av strenger
- `min` og `max` - verdier av nummer
- `pattern`

Ved å sette type og valideringsregler vil inputfeltet selv ofte kunne si ifra om det er gyldig eller ikke. Dette gjøres
ved at pseudo-class (tenk `:hover`) `:invalid` blir satt både på `form` og `input` elementet det gjelder. Vi kan derfor
enkelt style et inputfelt som ikke har gyldige verdier på denne måten:

```css
input:invalid {
  outline: 1px solid red;
}
```

Ved å bruke JavaScript kan vi få tak i et inputfelts `ValidityState` og på den måten finne ut hvilken feil et felt har
hvis det har flere. Vi kan også bruke JavaScript metoden `input.setCustomValidity('Feilmelding')` for å sette en egen
feilmelding.

### Egenskapen `required`

Den første egenskapen vi skal se på et `required`. `required` støttes av de fleste input typene, med untak av `color` og
`range` som har default verdier. Elementer med egenskapen `required` får også pseudo-class `:required`, og feltene som
ikke har den satt får tilsvarende pseudo-class `optional`. `reuired` gjør det en forventer. Den gjør at et felt som
mangler verdi blir satt til ugyldig.

#### Eksempel

```html
<label> Påkrevd felt: <input required /> </label>
```

<label>
    Påkrevd felt: <input required>
</label>

[Les mer om `required` i MDN web docs](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/required)

### Egenskapen `type`

Egenskapen `type` påvirker hvordan et input felt fungerer. Default input type er `text`. Noen av verdiene vil vise egne
komponenter / utvidede inputfelter som begrenser brukerens muligheter til verdier de kan skrive inn.

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

Typene `text`, `search`, `hidden`, `password`, `tel` kommer uten noen form for validering. En skulle kanskje tro
`password` var begrenset til `[a-zA-Z0-9_\-.]`, men det altså faktisk ikke det! `tel` blir ofte gruppert med `url` og
`email`, men telefonnummere har forskjellige formater og kan derfor ikke løses på noen god måte i et slikt felt.

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

Så kommer vi til de som kommer med et validerings pattern, `email` og `url`. Valdideringen for disse feltene er ofte nok
for det vi trenger. Men det er greit å vite at feks `email` godtar `dag@example` som er en gyldig epost, men ofte ikke
det vi er ute etter.

<label>
    <span class="label">Email: </span>
    <input type="email">
</label>

<label>
    <span class="label">URL: </span>
    <input type="url">
</label>

#### De med velgere

De typene vi har sett på til nå har vært fritekstfelter. `range`, `color` og `file` har derimot velgere som gjør at
brukeren ikke kan velge en ugyldig verdi.

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

#### De med delvis fritekst og velgere

I neste rekke kommer de feltene som lar brukeren skrive inn verdier selv, men som validerer de mens en skriver de inn.
`number` tillater også enkelte mattematiske symboler slik som `e`.

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

#### De som i seg selv ikke kan være feil i seg selv

`radio` og `checkbox` kan også settes til å være required, men vil ikke kunne være feil i seg selv. Men kan altså bli
feil om brukeren ikke velger et alternativ og prøver å submitte skjema.

<label>
    <span class="label">Radio: </span>
    <input type="radio">
</label>

<label>
    <span class="label">Checkbox: </span>
    <input type="checkbox">
</label>

`radio` og `checkbox` bør grupperes med et `fieldset` da dette forbedrer UU ved at skjermleserne forstår konteksten bedre. Andre felter som hører sammen kan med fordel også grupperes.

#### De som ikke har så mye å validere

De resterende input typene lar deg interagere med de, men tar ikke til seg verdier.

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

[Les mer om `type` i MDN web docs](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input#%3Cinput%3E_types)

### Egenskapene `minLength` og `maxLength`

Egenskapene `minLength` og `maxLength` sier hvor mange tegn et bruker skal få skrive inn i et `input` eller `textarea`.
`maxLength` forhinder brukeren fra å skrive inn mer enn max antall tegn, mens `minLength` vil gjøre feltet ugyldig fra
det første tegnet er skrevet inn, til `minLength` er nådd.

#### Eksempel

```html
<label> Inputfelt for 3-5 tegn: <input minlength="3" maxlength="5" /> </label>
```

<label>
    Inputfelt for 3-5 tegn: <input minLength="3" maxLength="5">
</label>

[Les mer om `minLength` i MDN web docs](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/minlength)

[Les mer om `maxLength` i MDN web docs](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/maxLength)

### Egenskapene `min` og `max`

Egenskapene `min` og `max` sier noe om den minste og største godkjente verdien til et `input`. Nettleseren vil prøve å
forhindre brukeren fra å skrive inn ugyldige verdier og sette `input` til `:invalid` hvis verdien går utenfor `min` og
`max`.

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

[Les mer om `min` i MDN web docs](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/min).

[Les mer om `max` i MDN web docs](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/max)

### Egenskapen `pattern`

Den siste egenskapen vi kan sette for å få et inputfelt til å bli validert er `pattern`.

#### Eksempel

```html
<label>
  Inputfelt som kun kan inneholde engelske bokstaver:
  <input pattern="[a-zA-Z]*" />
</label>
```

<label>
    Inputfelt som kun kan inneholde engelske bokstaver: <input pattern="[a-zA-Z]*" />
</label>

[Les mer om `pattern` i MDN web docs](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/pattern)

### Metoden `setCustomValidity`

Vi kan også bruke metoden `setCustomValidity` får å gjøre et input felt ugyldig på samme måte som blir gjort hvis input
feltene feiler av en av de innebygde attributtene. Men feilmeldingen her dukker ikke opp før en prøver å sende inn  skjema. Vi pakker derfor inputfeltet i et form og legger til en knapp for å sende inn dataene. 

`setCustomValidity` vil også la nettleseren scrolle til feltet som er ugyldig.

#### Eksempel setCustomValidity

```html
<form>
  <label>
    Input:
    <input onchange="validateRiktig(this)" />
  </label>
  <button>Submit</button>
</form>

<script>
  function validateRiktig(input) {
    error = "";
    if (input.value != "Riktig") {
      error = 'Input teksten er ikke "Riktig"';
    }
    input.setCustomValidity(error);
  }
</script>
```

<br>
<form>
    <label>
        Input:
        <input onchange="validateRiktig(this)">
    </label>
    <br>
    <button>Submit</button>
</form>

<script>
    function validateRiktig(input) {
        error = "";
        if (input.value != "Riktig") {
            error = 'Input teksten er ikke "Riktig"'
        }
        input.setCustomValidity(error);
    }
</script>

[Les mer om `setCustomValidity` i MDN we
docs](https://developer.mozilla.org/en-US/docs/Web/API/HTMLObjectElement/setCustomValidity)

## Brukeropplevelse

Vi har nå vært gjennom det vi trenger å vite om innebygged validering av input til å skape en god brukeropplevelse!

For å skape en god brukeropplevelse ved input validering er det viktig å tenke på:

1. Hvor en viser feilene
2. Vise feilmeldingene på riktig tidspunkt
3. Bruke fornuftige farger
4. Bruk tydelig språk

### 1. Hvor en viser feilene

Det er mange måter å vise feilmeldinger på. Jeg vil anbefale og vise feilmeldingen tett tilknyttet inputfeltet det gjelder, da enten til høyre eller under input feltet som er ugyldig. Hvis formet er langt bør en scrolle til der feilen er når brukeren prøver å sende inn data.

#### Til høyre

<form>
    <label>
        <span class="label">Navn: </span>
        <input value="Dag Frode">
    </label>
    <br>
    <label>
        <span class="label">Epost: </span>
        <input type="email" value="ikke en gyldig epost">
        <span class="error">Venligst skriv inn en gyldig epost</span>
    </label>
    <br>
    <label>
        <span class="label">Adresse: </span>
        <input value="Hakkebakkestien">
    </label>
</form>

#### Under

<form>
    <label>
        <span class="label">Navn: </span>
        <input value="Dag Frode">
    </label>
    <br>
    <label>
        <span class="label">Epost: </span>
        <input type="email" value="ikke en gyldig epost">
        <span class="error-next-line">Venligst skriv inn en gyldig epost</span>
    </label>
    <br>
    <label>
        <span class="label">Adresse: </span>
        <input value="Hakkebakkestien">
    </label>
    <br>
    <button>Send inn</button>
</form>

### 2. Vise feilmeldingene på riktig tidspunkt

Det er slitsomt for brukeren å få feilmeldingen kastet i fleisen som i eksemplene over. Det er derfor lurt å vente til en bruker trykker ut av et felt med å vise feilmeldingene. Dette kan gjøres med litt enkel JavaScript og CSS. 

Hvis det er snakk om et komplisert felt som skal valideres kan en kjøre validering noen ms etter at brukeren har sluttet å skrive.

```html
<input
  type="email"
  class="timed-error"
  oninput="this.setAttribute('show-error', false)"
  onblur="this.setAttribute('show-error', true)"
/>
```

```css
.timed-error {
  display: none;
  color: red;
}

input[show-error="true"]:invalid {
  outline: 1px solid red;
}

input[show-error="true"]:invalid + .timed-error {
  display: inline;
}
```

<form>
    <label>
        <span class="label">Navn: </span>
        <input value="Dag Frode">
    </label>
    <br>
    <label>
        <span class="label">Epost: </span>
        <input type="email" oninput="this.setAttribute('show-error', false)"
            onblur="this.setAttribute('show-error', true)" />
        <span class="timed-error">Venligst skriv inn en gyldig epost</span>
    </label>
    <br>
    <label>
        <span class="label">Adresse: </span>
        <input value="Hakkebakkestien">
    </label>
    <br>
    <button>Send inn</button>
</form>

### 3. Bruke fornuftige farger

Her vil jeg presisere at det er fornuftig å benytte en farge en forbinder med feil, feks rød med feilmeldingen og en annen farge for når ting går bra, feks grønn. Det er lurt å få feilmeldingene til å skille seg ut ved feks å gjøre teksten rød slik at brukeren får med seg at det er en feil. Det kan gjerne brukes en `*` eller en `x` også for å markere at dette er ekstra informasjon til inputfeltet slik at de 5-7% av oss som sliter med farger også får det med seg. Og tilsvarende for når ting går bra.

<form>
    <label>
        <span class="label">Navn: </span>
        <input value="Dag Frode">
    </label>
    <br>
    <label>
        <span class="label">Epost: </span>
        <input type="email" show-error="true" oninput="this.setAttribute('show-error', false)"
            onblur="this.setAttribute('show-error', true)" value="ikke en epostaddresse" />
        <span class="timed-error">*Venligst skriv inn en gyldig epost</span>
    </label>
    <br>
    <label>
        <span class="label">Adresse: </span>
        <input value="Hakkebakkestien">
    </label>
    <br>
    <button>Send inn</button>
</form>

### 4. Bruk tydelig språk

Det siste det er viktig å tenke på for å skapeen god brukeropplevelse er å gi en god tilbakemelding når noe er feil. Bruk positivt språk som oppmuntrer brukeren. Prøv å gjøre feilmeldingene beskrivende slik at det er lett å forstå hva som gikk feil


## Løsningsforslag

Vi bruker samme stylingen som tidligere

```css
.timed-error {
  display: none;
  color: red;
}

input[show-error="true"]:invalid {
  outline: 1px solid red;
}

input[show-error="true"]:invalid + .timed-error {
  display: inline;
}
```

Vi lager en funksjon for å validere et av inputfeltene våre. Vi targeter labelen da den får beskjed når inputfeltet den er knyttet til oppdaterer seg, altså når `input` har et event av type `blur`. Ved å targete labelen får vi også tak i feilmeldingsfeltet slik at vi får oppdatert både inputfeltet og error feltetsamtidig.

```js
function validateUsername(label) {
    var input = label.querySelector("input");
    var errorSpan = label.querySelector("span.timed-error");
    error = "";
    if (!/\d/.test(input.value) ) {
        error = `*${input.value} er alt brukt. Hva med ${input.value}1?`
    }

    input.setCustomValidity(error);
    errorSpan.innerText = error;
}
```

Vi har pakket feltene i et `fieldset` for å gruppere de.

```html
<form>
    <fieldset>
        <legend>Personalia:</legend>
        <label onchange="validateUsername(this)">
            <span class="label">Brukernavn: </span>
            <input show-error="false" oninput="this.setAttribute('show-error', false)"
                onblur="this.setAttribute('show-error', true)">
            <span class="timed-error">derp</span>
        </label>
        <label>
            <span class="label">Email: </span>
            <input type="email" show-error="false" oninput="this.setAttribute('show-error', false)"
                onblur="this.setAttribute('show-error', true)">
            <span class="timed-error">*Vennligst skriv inn en gyldig epost</span>
        </label>
        <button>Submit</button>
    </fieldset>
</form>
```



<form>
    <fieldset>
        <legend>Personalia:</legend>
        <label onchange="validateUsername(this)">
            <span class="label">Brukernavn: </span>
            <input show-error="false" oninput="this.setAttribute('show-error', false)"
                onblur="this.setAttribute('show-error', true)">
            <span class="timed-error">derp</span>
        </label>
        <label>
            <span class="label">Email: </span>
            <input type="email" show-error="false" oninput="this.setAttribute('show-error', false)"
                onblur="this.setAttribute('show-error', true)">
            <span class="timed-error">*Vennligst skriv inn en gyldig epost</span>
        </label>
        <button>Submit</button>
    </fieldset>
</form>

<script>
    function validateUsername(label) {
        var input = label.querySelector("input");
        var errorSpan = label.querySelector("span.timed-error");
        error = "";
        if (!/\d/.test(input.value) ) {
            error = `*${input.value} er alt brukt. Hva med ${input.value}1?`
        }

        input.setCustomValidity(error);
        errorSpan.innerText = error;
    }
</script>

kilde:
https://medium.com/@andrew.burton/form-validation-best-practices-8e3bec7d0549
