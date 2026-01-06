# Skrapforhandler Register

Et registreringssystem for skrapforhandlere utviklet som del av et emne ved Høyskolen Kristiania.

## Om prosjektet
Applikasjonen lar brukeren registrere, hente, oppdatere og slette informasjon om skrapforhandlere. Data lagres i en SQL-database og hentes ut via JDBC.

## Teknologier
- Java (objektorientert)
- SQL / MySQL
- JDBC for databasetilkobling

## Funksjonalitet
- Legge til nye skrapforhandlere
- Vise registrerte forhandlere
- Oppdatere eksisterende oppføringer
- Slette oppføringer fra databasen

## Struktur
Prosjektet følger objektorienterte prinsipper med separate klasser for modeller, databasehåndtering og brukergrensesnitt.


Innkapsling
I løsningen min har jeg brukt innkapsling gjennom hele systemet. I Vehicle klassen har jeg
private attributter som vehicleID, brand og model som kun kan nås gjennom getter og setter
metoder. Dette beskytter dataene og hindrer at objektenes tilstand kan endres på feil måter.
I Scrapyard klassen har jeg også private attributter med tilhørende getter og setter metoder.
Dette gir full kontroll over hvordan dataene brukes og endres.
I DbScrapyard klassen er alle SQL statements definert som private static final konstanter, og
database tilkoblingen er privat. Dette sikrer at sensitive database operasjoner ikke kan
påvirkes utenfra.

Arv og Polymorfi
Jeg har laget et klassehierarki med Vehicle som abstrakt superklasse og FossilCar,
ElectricCar og Motorcycle som subklasser. Vehicle klassen har alle felles egenskaper som
alle kjøretøy har, mens hver subklasse legger til sine egne attributter som fuelType for
fossilbiler, batteryCapacity for elbiler og engineCapacity for motorsykler.
Polymorfi brukes i insertVehicle metoden hvor jeg bruker instanceof for å finne ut hvilken
type kjøretøy det er og kalle riktig insert metode. I FileReader lagres alle kjøretøy som
Vehicle objekter i samme ArrayList, men når toString kalles får vi automatisk riktig utskrift
basert på hvilken type objekt det er.
Den abstrakte getVehicleType metoden sikrer at alle subklasser må lage sin egen versjon.
Dette gjør systemet utvidbart så nye kjøretøytyper kan legges til enkelt.

Utfordringer og løsninger
Den største utfordringen var å få fillesingen til å fungere med vehicles.txt. Filen har en
blanding av tall, tekst og separatorer, så det var viktig å håndtere Scanner objektet riktig. Jeg
løste dette ved å alltid bruke scanner.nextLine etter hver nextInt for å unngå problemer med
linjeskift.

En annen utfordring var å håndtere duplikater i databasen ved gjentatte kjøringer. Jeg løste
dette med INSERT IGNORE i alle SQL statements som gjør programmet robust.
For toString implementeringen valgte jeg å la hver subklasse vise sine egne attributter først,
så superklassens toString. Dette gir oversikt over alle data og viser arvehierarkiet.
Jeg valgte også å beholde ID utskriften i FileReader for å vise at dataene ble lest riktig. Dette
er nyttig for debugging og gir brukeren bekreftelse på at importen fungerte.
Valgfri funksjonalitet
Som valgfri funksjonalitet laget jeg "Se kjøretøy per skraphandlested" som grupperer alle
kjøretøy etter hvilket skraphandlested de tilhører. Denne funksjonen krever database
kommunikasjon med flere tabeller og sammenligning av ScrapyardID for å vise riktig
gruppering.

Forutsetninger
Jeg forutsatte at vehicles.txt følger formatet som er beskrevet i oppgaven. Programmet
håndterer både import av nye data og data allerede finnes i databasen.
Jeg valgte engelsk meny som vist i oppgaveteksten, men brukte norsk "Kjørbar" i toString
metodene for konsistens med eksemplene.
Database tilkoblingen er konfigurert gjennom scrapyard.properties filen hvor du kan
legge inn sitt eget brukernavn og passord for å tilpasse det til sin database.
ArrayList brukes for å lagre kjøretøy fordi det gir enkel tilgang til alle elementene og bevarer
rekkefølgen når de skal vises.
