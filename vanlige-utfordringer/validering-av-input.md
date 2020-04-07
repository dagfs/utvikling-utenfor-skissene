<style>
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

Videre i denne artikkelen tar vi utgangspunkt i at den beste fremgangsmåten er å gjøre validering av input i frontend også, og ser derfor på hvordan dette gjøres på en god måte, både teknisk og for brukeropplevelsen.

## Hvordan validere

[HTML5 inneholder nå mange funksjoner for å validere input](https://developer.mozilla.org/en-US/docs/Learn/Forms/ Form_validation#Using_built-in_form_validation). Det har blitt så bra at en i stor grad kan løse validering med disse. Hvis en kombinerer denne valideringen med [input typer](https://developer.mozilla.org/en-US/docs/Learn/Forms/HTML5_input_types) kan en skape en god brukeropplevse med minimal bruk av JavaScript.

De innebygde egenskapene(atributt) som brukes for validering er:

- `required`
- `minlength` og `maxlength` - lengde av strenger
- `min` og `max` - verdier av nummer
- `type` - button, checkbox, color, date, datetime-local, email, file, hidden, image, month, number, password,
  radio, range, reset, search, submit, tel, text, time, url, week.

Ved å sette type og valideringsregler vil inputfeltet selv ofte kunne si ifra om det er gyldig eller ikke. Dette gjøres ved at pseudo-class (tenk `:hover`) `invalid` blir satt både på `form` og `input` elementet det gjelder.

Vi kan derfor enkelt style et inputfelt som ikke har gyldige verdier på denne måten:

```css
input:invalid {
  outline: 1px solid red;
}
```

### `required`

Den første egenskapen vi skal se på et `required`. `required` støttes av de fleste input typene, med untak av `color` og `range` som har default verdier. Elementer med egenskapen `required` får også pseudo-class `:required`, og feltene som ikke har den satt får tilsvarende pseudo-class `optional`. `reuired` gjør det en forventer. Den gjør at et felt som mangler verdi blir satt til ugyldig.

#### Eksempel

```html
<label> Påkrevd felt: <input required /> </label>
```

<label>
    Påkrevd felt: <input required>
</label>

[Les mer om `required` i MDN we docs](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/required)

### `minLength` og `maxLength`

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

### `min` og `max`

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

### `type`

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

## Brukeropplevelse

https://medium.com/@andrew.burton/form-validation-best-practices-8e3bec7d0549

For å skape en god brukeropplevelse ved input validering er det viktig å tenke på:

### 1. Hvor en viser feilene

### 2. Vise feilmeldingene på riktig tidspunkt

### 3. Bruke fornuftige farger

### 4. Bruk tydelig språk

---

## setCustomValidity

<label>
    <input type="number" oninvalid="setCustomValidity('Venligst fyll inn et tall mellom 5 og 20000')"
        oninput="setCustomValidity('')" min="5" max="20000" value="1">
    <span></span>
</label>
https://developer.mozilla.org/en-US/docs/Web/API/HTMLSelectElement
