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

input[showError="true"]:invalid+.timed-error {
    color: red;
    display: inline;
}
</style>
<div>

# Validering av input

Det er hovedsaklig to strategier for valigering av input. Validering i backend, og validering i frontend. Backend
validering må gjøres uansett, og vil kunne gi brukeren gode tilbakemeldinger, men vil kreve at brukeren lagrer data
før de får se om noe er feil. Validering i frontend gir brukeren tilbakemelding med en gang og gir ofte en bedre
brukeropplevelse hvis det blir gjort på en god måte. Det er også mulig å kombinere ved å sende inn dataene brukeren
skriver inn underveis for å returnere eventuelle feil.

Videre i denne artikkelen tar vi utgangspunkt i at den beste fremgangsmåten er å gjøre validering av input i
frontend også, og ser derfor på hvordan dette gjøres på en god måte.

Validering består av hvordan teknisk løse valideringen og hvordan vise feilene.

## Hvordan validere

[HTML5 inneholder nå mange funksjoner for å validere
input](https://developer.mozilla.org/en-US/docs/Learn/Forms/Form_validation#Using_built-in_form_validation). Det har
blitt så bra at en i stor grad kan løse validering med disse. Hvis en kombinerer denne valideringen med [input
typer](https://developer.mozilla.org/en-US/docs/Learn/Forms/HTML5_input_types) kan en skape en god brukeropplevse med minimal bruk av JavaScript.

De innebygde valideringsmetodene er:

- `required`
- `minlength` og `maxlength` - lengde av strenger
- `min` og `max` - verdier av nummer
- `type` - button, checkbox, color, date, datetime-local, email, file, hidden, image, month, number, password, radio, range, reset, search, submit, tel, text, time, url, week.

Ved å sette type og valideringsregler vil inputfeltet selv ofte kunne si ifra om det er gyldig eller ikke.

### `required`

<label>
Påkrevd felt: <input required>
<span class="error">*</span>
</label>

### `minLength` og `maxLength`

### `min` og `max`

### `type`

<label>
Epost: <input type="email">
    <span class="error">Venligst skriv inn en gyldig epost</span>
</label>
<br>
<br>
<form>
<label>
Epost: <input type="email" required showError="false" oninput="this.setAttribute('showError', false)"
        onblur="this.setAttribute('showError', true)">
    <span class="timed-error">Vennligst skriv inn en gyldig epostadresse</span>
</label>
<button>Lagre</button>
</form>

https://medium.com/@andrew.burton/form-validation-best-practices-8e3bec7d0549

## Brukeropplevelse

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

</div>
