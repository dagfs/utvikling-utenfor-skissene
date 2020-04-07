# Form

Jeg har intrykk av at vi kan bli flinkere til å bruke forms. Det er vanlig å se at en kobler `onClick` på en knapp og henter ut verdier fra inputfelter som ikke henger sammen. Det er i det minste jeg har tatt meg selv i å gjøre ved en del anledninger.

Det å pakke inputfeltene våre inn i et `<form>` gir oss en del fordeler. Det lar oss knytte om formet kan sendes inn til om _submit_ input, eller knappet `<button> === <button type="submit">`. En får også muligheten til å lagre et skjema ved å trykke _enter_, noe som blir skrudd av hvis _submit_ elementet vårt er _disablet_

```
<form>
    <input />
    <button>Lagre</button>
</form>
```

<form>
    <input />

    <button>Lagre</button>

</form>

```
<form>
    <input />

    <button disabled>Lagre</button>
</form>
```

<form>
    <input />

    <button disabled>Lagre</button>

</form>
