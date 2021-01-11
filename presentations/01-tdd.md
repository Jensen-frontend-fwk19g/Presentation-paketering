<!-- .slide: class="intro" -->

# Paketering, leverans och uppföljning
## TDD
###### david.andersson@zocom.se

Note: tryck S för att öppna speaker view

---

## Idag

* reveal-md och helper.zocom.io
* Vad är TDD?
* Varför ska man skriva tester?
* Konfigurera och köra tester


---
<!-- .slide: class="basic blue" -->
### Vad är reveal-md?

Ett npm-paket för att skapa presentationer. Se <code>README.md</code> för dokumentation.

Tryck "o" för att se flera slides på samma gång.

### Vad är Helper?

Ett verktyg som ZoCom utvecklar för att underlätta distansundervisning. Skriv ditt namn och vilket rum du ska ansluta till. Välj sedan färg för att tala om hur det går för dig.

* **grön** - klar, läget under kontroll
* **gul** - jobbar på, men behöver ingen hjälp just nu
* **röd** - sitter fast, behöver hjälp just nu

---
![Varför testdriven utveckling behövs](img/need-of-test-driven-development.jpg)


---
## Linux kernel size
Storleken på källkoden för operativsystemet Linux, från 1995 till 2019
![Linux kernel size](img/kernel-size.png)

---
*Velocity* är hur snabbt teamet kan lägga till ny funktionalitet.

Ju längre man kommer i ett projekt, desto mer komplicerat blir det. Man lägger mer och mer tid på att fixa buggar. Det blir svårare att lägga till ny funktionalitet, för att koden är så rörig.

Detta vill TDD ändra på.


---

## TDD

TDD == Test-Driven Development (testdriven utveckling)

TDD kommer från eXtreme Programming, 1999. Det går ut på att skriva funktioner som testar koden innan man skrivit den.

I stället för att manuellt testa att ett program fungerar korrekt skriver man testfall (*test cases*). Moderna testramverk kör alla testfall automatiskt.

**"Red, green refactor"** är en minnesregel som sammanfattar hur man skriver enhetstest enligt TDD.
+ Red - skriv ett test som failar, ett "rött" test
+ Green - skriv minsta möjliga kod som får testet att lyckas, bli "grönt"
+ Refactor - förbättra koden

---
![TDD circle](img/red-green-refactor.png)
---

## Hur gör man?

1. Börja med **specifikationen**. Diskutera inom teamet vad appen ska kunna. Det kan hända att kunden ger en kravspec, men man måste ändå diskutera den inom teamet, så att alla är överens om vad som ska göras.
2. När man har en hyfsad bild över vilka krav som finns på appen, så skriver man ner dem som **kort i Trello**. (gärna som user stories)
3. Sedan tar man ett kort och **börjar skriva testfall**. Testfallen ska svara på frågan: *uppfyller koden kraven?* Nu är det ok att skriva testfallens kod.
4. Allra sist skriver man **implementationen**, så att testfallet kan bli grönt.
---

## Case study: ränta (1/3)

Vi ska skriva en funktion som räknar ut ränta, som ska användas på en bank. <br>Ett utkast till specifikation:
1. funktionen ska anropas med två parametrar, räntesats och antal år
2. räntesats ska vara ett tal `1 <= x < 2`
3. antal år ska vara ett positivt heltal
4. om antal år är `0` ska talet `1` returneras
---

## Case study: ränta (2/3)

Testfall:

+ it should throw an error if `interest` has a bad value
+ it should throw an error if `years` has a bad value
+ it should throw an error if wrong number of parameters
+ it should return `1` if `years == 0`
+ it should return correct interest rate otherwise

---

## Case study: ränta (3/3)

Börja med *ett* testfall!

#### 4: it should return `1` if `years == 0`

```javascript[1-2|4|6]
let years = 0, rate = 1.075;
let expected = 1;

let actual = calculateInterest(years, rate);

if( expected !== actual ) throw new Error('Test #4 failed')
```

Nu får du skriva *minsta möjliga kod* som gör att testfallet uppfylls.

Note: Arrange, Act, Assert

---

## Case study: boka tvättid (1/3)

Vi vill bygga en app som vi kan använda för att boka tvättid.
Ett utkast till specifikation:
1. man ska kunna logga in, så att systemet vet vilken användare man är
2. systemet ska visa vilka tider som är lediga
3. inloggad användare ska kunna boka ett tvättpass
4. användare kan inte boka fler än ett tvättpass
5. ett tvättpass börjar kl 9, 13, 17 och håller på i exakt 4 timmar
6. inloggad användare kan avboka sitt tvättpass

Har vi fått med allt?

---

## Case study: boka tvättid (2/3)

Testfall:
+ it should log in user with correct apartment number
+ it should display all the free time slots
+ it should not book a time slot if start time is incorrect
+ it should not book a time slot if time slot is occupied
+ it should not book a time slot if at least one other booking exist
+ it should book a time slot if start time is correct, time slot is free and no previous booking exist
+ a booked time slot should be exactly four hours long
+ osv...

---
## Case study: boka tvättid (3/3)
```js[1-4|6-7|9|11-14]
function bookTimeSlot(startTime) {
    // implementation goes here
    // let's assume it should return TRUE if booking is successful
}

let badStartTime = '10:00';   // only 9, 13, 17 allowed
let expected = false;  // we expect booking to fail

let actual = bookTimeSlot(badStartTime);

if( actual !== expected )
    console.log('test FAILED (booked bad start time)');
else
    console.log('test SUCCESSFUL (did not allow bad booking)');
```
---
## Jest
Ett testramverk som hjälper oss skriva tester. Exempel:
```javascript
// importera koden som ska testas
const exampleFunction = require('example.js');

// utgå från user stories i kravspec
it('should return true for correct input', () => {
    // arrange - förbered testet
    let expected = true;

    // act - anropa funktioner
    let actual = exampleFunction();

    // assert - kontrollera resultatet
    expect(actual).toBe(expected);
})
```
https://jestjs.io/docs/en/getting-started.html

---
<!-- .slide: class="code" -->

## Lets code!
Vi arbetar med övningarna, stycke 1.
