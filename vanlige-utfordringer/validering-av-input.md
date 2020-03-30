<div>

# Validering av input

Det er hovedsaklig to strategier for valigering av input. Validering i backend, og validering i frontend. Backend validering må gjøres uansett, og vil kunne gi brukeren gode tilbakemeldinger, men vil kreve at brukeren lagrer data før de får se om noe er feil. Validering i frontend gir brukeren tilbakemelding med en gang og gir ofte en bedre brukeropplevelse hvis det blir gjort på en god måte. Det er også mulig å kombinere ved å sende inn dataene brukeren skriver inn underveis for å returnere eventuelle feil.

Videre i denne artikkelen tar vi utgangspunkt i at den beste fremgangsmåten er å gjøre validering av input i frontend også, og ser derfor på hvordan dette gjøres på en god måte.

Validering består av hvordan teknisk løse valideringen og hvordan vise feilene.

## Brukeropplevelse

For å skape en god brukeropplevelse ved input validering er det viktig å tenke på:

1. Hvor en viser feilene
2. Vise feilmeldingene på riktig tidspunkt
3. Bruke fornuftige farger
4. Bruk tydelig språk

## Hvordan validere

[HTML5 inneholder nå mange funksjoner for å validere input](https://developer.mozilla.org/en-US/docs/Learn/Forms/Form_validation#Using_built-in_form_validation). Det har blitt så bra at en i stor grad kan løse validering med disse. Hvis en kombinerer denne valideringen med [input typer](https://developer.mozilla.org/en-US/docs/Learn/Forms/HTML5_input_types) kan en skape en god brukeropplevse uten å bruke JavaScript.

De innebygde valideringsmetodene er:

- `required`
- `minlength` og `maxlength` - lengde av strenger
- `min`og `max` - verdier av nummer
- `type` - button, checkbox, color, date, datetime-local, email, file, hidden, image, month, number, password, radio, range, reset, search, submit, tel, text, time, url, week.

Ved å sette type vil inputfeltet selv

<input required >

https://medium.com/@andrew.burton/form-validation-best-practices-8e3bec7d0549

</div>
