# Huwelijksplanner
In de volgende stappen worden de benodigd API calls per stap doorgelicht


## Scherm 0: Introductie

[Bekijk prototype ](https://huwelijk.utrecht.eend.nl/docs/site/huwelijksplanner/index.html)

![img.png](img.png)

**Introductie**:
- verwachtingen
  - type trouwen dat kan
  - datum kiezen
  - locatie kiezen
- wat heb je nodig?
  - DigiD van jou en partner (niet beiden toegang met DigiD? ga naar de balie)
  - iDEAL (tenzij gratis)
  - gegevens van getuigen
  - 
**De voorwaarden worden niet expliciet vooraf meegedeeld**:
- partners zijn beiden 18 jaar (op trouwdatum)
- niet al getrouwd
- verklaring 2e graads bloedverwantschap

**Opmerking**:
kun je ook een ambtenaar kiezen?

## Scherm 1: Huwelijk of geregistreerd partnerschap?

Bekijk prototype

3

•
•

•
Keuze verbintenis:
ik wil trouwen
ik wil een geregistreerd partnerschap
POST API:

Deze gegevens worden opgestuurd:
identifier voor keuze tussen trouwen of geregistreerd partnerschap
Er zijn afspraken nodig over de identifier van het type.

## Scherm 2: datum kiezen

Bekijk prototype

4

•

•
•
•

•
•
•
•
Rekening houden met:
Datum moet zijn nadat beide personen 18 zijn (persoonsgegevens zijn echter
nog niet beschikbaar in deze stap)
Datum alleen op beschikbare datums.
Datum moet zijn na vandaag.
Datum is maximaal uiterste datum X in toekomst.
GET API:
Deze gegevens zijn nodig om de pagina te tonen.
Request parameters:
gemeente (verplicht of afwezig en standaard Utrecht?)
type (optioneel, anders alle types)
begindatum (optioneel, anders vanaf vandaag + minimumtijd - nu 14 dagen)
einddatum (optioneel, anders uiterste datum in toekomst - nu maximaal 1 jaar
in de toekomst)

5

•
•

•
•
•

•
•
•

•
•

•
Response:
lijst met unieke combinaties van datum + tijdstip + type
extra velden: locatie
Afspraken over identifiers:
gemeente
type
"geregistreerd partnerschap" - "flitshuwelijk" - "gratis huwelijk" - "eenvoudig
huwelijk" - "uitgebreid huwelijk"

POST API

Deze gegevens worden vanaf deze stap naar de server verstuurd.
datum
tijdstip
type

## Scherm 3: overzicht keuze, door naar DigiD

Bekijk prototype
Overzicht:
keuze product
datum en tijdstip
Betaal om melding te maken van voorgenomen huwelijk en tijd en locatie te
reserveren.
Opmerking:
op dit punt kun je al een QR-code tonen zodat de partner ook kan inloggen,
zodat je het echt "tegelijk en samen doet"

6

•
•
•
•

•
•

GET API:
Deze informatie is nodig om de pagina te tonen:
gekozen type
product prijs
locatie
gekozen tijdstip

POST API:

Een POST naar een URL met de intentie om te redirecten naar DigiD. Gegevens die
de server moet weten:
naar welke URLs geredirect moet worden wanneer DigiD een success is
de URL van de foutmelding pagina

Unhappy flow

DigiD is down.

### Scherm: inloggen met DigiD

Bekijk prototype

7

Scherm: inloggen bij DigiD

Bekijk prototype

8

•
•

•
•
## Scherm 4: contactgegevens invullen en gegevens controleren
Deze pagina toont ter controle:
Overzicht persoonsgegevens
Overzicht adresgegevens:
Geeft de mogelijkheid voor persoon 1 voor het aanvullen van contactgegevens:
telefoonnummer (optioneel, voor bijvoorbeeld Doven)
e-mailadres
Bekijk prototype

9

10

•
•
•
•
•
•
•
•
•
•
•
•
•
•
•

•
•

GET API:
De volgende gegevens zijn nodig om de pagina te tonen:
Persoonsgegevens
Burgerservicenummer
Aanhef
Voornamen
Tussenvoegsel(s)
Achternaam
Burgelijke staat
Geboortedatum
Geboorteplaats
Indicatie curateleregister
Adresgegevens
Straatnaam
Huisnummer (+ toevoegingen)
Postcode
Woonplaats

POST API:
Deze pagina verstuurt het volgende naar de server:
telefoonnummer - optioneel
e-mailadres - verplicht

## Scherm 5: partner uitnodigen

Bekijk prototype

11

•

•
Opmerkingen:

kan misschien met QR code in plaats van link in e-mail, geen latency, geen junk-
mail risico

persoonsgegevens van partner komen uit DigiD, ipv zelf ingevuld

### Scherm: getuigen aanmelden

Bekijk prototype

12

13

•

•
•
•

Scherm: controles uitgevoerd

Bekijk prototype

Happy flow: alle controles zijn geslaagd
Unhappy flow
één of meerdere checks zijn niet geslaagd
GET API:
Deze gegevens zijn nodig om de pagina te tonen:
lijst van checks
beschrijving controle
geslaagd / niet geslaagd

14
POST API

Deze pagina verstuurt zelf geen informatie, maar de server moet wel weten naar
welke URLs geredirect moet worden na een succesvolle of mislukte betaling.

### Scherm: wachten op partner

Bekijk prototype

Unhappy flow

Reservering verlopen.

15

•
•
•
•

•
•

GET API

naam partner
e-mailadres partner
tijdstip verlopen reservering
status reservering: verlopen / niet verlopen
PUT API
Gegevens om bestaande informatie aan te passen.
naam partner
e-mailadres partners

### Scherm: betaling gelukt

Bekijk prototype

16

17

•

•

•
•
•
•

GET API

Voor feedback:
status betaling: gelukt / niet gelukt
Voor overzicht gegevens:

E-mail: bevestiging melding van huwelijk

Mail naar beide partners.
Benodigde gegevens voor e-mail template:
naam 1
naam 2
datum en tijdstip
locatie

18

•
•
•
•
type
laatste datum getuigen melden
laatste datum extra's bestellen
URL om reservering te bekijken

### Scherm: getuigen aanpassen

Bekijk prototype

19

20

•

### Scherm: extra bestellen

Bekijk prototype

Bijvoorbeeld, een huwelijksboekje bestellen en gelijk betalen.
Opmerking: is dit nog achteraf te wijzigen? Betaal je dan het bedrag voor het
nieuwe product, en wordt buiten de planner om het reeds betaalde bedrag
teruggestort? Of moet je dan kunnen bijbetalen? Dan moet de server ook weten
hoeveel reeds betaald is.

GET API:
Lijst extra producten, met varianten:
product naam (bijv. trouwboekje)

21

•
•
•
•
•

•
product ID
mogelijke varianten (rood leder / zwart leder / blauw textiel / naturel karton):
variant titel
variant prijs
variant ID

POST API:
één of meerdere product ID + mogelijk variant ID
Unhappy flow

Storing in betaalservice.

### Scherm: annuleer reservering

Opmerking: de knop "Annuleer reservering" is een beetje onfortuinlijk, omdat
"Annuleer" ook vaak betekent "Ik wil die schermpje niet, doe niets". Misschien
duidelijker om "Annuleren" (secondary button) en "Geen huwelijk reserveren"
(primary button) te doen, of iets dergelijks.

###Scherm: bevestiging annulering

Bekijk prototype

22

•
•

E-mail: bevestiging annulering

e-mail aan partners
andere e-mail aan getuigen
Details nader te bepalen.

Techniek

De pagina moet voldoen aan DigiD richtlijnen rondom een strikte Content-
Security-Policy . We willen kiezen om een server-side rendering te doen met

React, zodat we een nonce kunnen genereren voor elke pagina.

23

•

•

•

•

•

•

•

•

•

