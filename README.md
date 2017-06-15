# Born digital API
***work in progress***

Content Partners (CP's) die digitaal geboren materiaal aanleveren kunnen de status hiervan opvragen aan de hand van een API.
Dit rapport geeft aan wat gearchiveerd of opnieuw aangeleverd moet worden.

Er wordt gebruik gemaakt van SSL client certificaten om deze extern te kunnen bevragen.
Hieronder wordt uitgelegd hoe dit opgezet wordt.

Contacteer support@viaa.be bij vragen of opmerkingen.

# Certificaat bekomen

1) De eerste stap is om een certificate signing request (csr) te genereren.
Enkele parameters (die tussen accolades staan) moeten ingevuld worden door de gebruiker:
- {organisatie-ID}: dit is de organisatie ID. Dit krijg je van VIAA.
- {naam}: een gebruikersnaam
- {email-adres}: uw email-adres

Open een terminal en run volgend commando met de parameters ingevuld:
```bash
openssl  req -out viaa_ingest.csr -new -newkey rsa:2048 -keyout viaa_ingest.key -subj '/DC=ingest/O={organisatie-ID}/CN={naam}/emailAddress={email-adres}'
```

2) Stuur het bestand `viaa_ingest.csr` terug naar de VIAA-contactpersoon.
3) U krijgt het certificaatbestand `viaa_ingest.crt` van VIAA.

# API bevragen

## Browser

1) Om de API te bevragen via een browser moet eerst een PK12-versie van het certificaat gemaakt worden.
Dit kan met volgend commando:
```bash
openssl pkcs12 -export -in viaa_ingest.crt -inkey viaa_ingest.key -out viaa_ingest.p12
```
2) Vervolgens moet dit geïmporteerd worden in uw favoriete browser, bv.:
- voor Chrome: Settings > Show advanced settings > Manage certificates > File (inside the taskbalk) > Import item
- voor Firefox: Preferences > Advanced > Certificates > Check certificates > Import

3) Ga nu naar https://api.viaa.be/ingest (productie-omgeving) of https://api-qas.viaa.be/ingest (test-omgeving).

## Terminal (cURL)

```bash
curl --key viaa_ingest.key --cert viaa_ingest.crt "https://api-qas.viaa.be/ingest"
```
# Documentatie

Documentatie kan opgevraagd worden via https://api.viaa.be/ingest/console (productie-omgeving) of https://api-qas.viaa.be/ingest/console (test-omgeving).
Let op: hiervoor moeten vorige stappen doornomen geweest zijn.

Voorbeeld request: https://api-qas.viaa.be/ingest?startIndex=0&nrOfResults=10&local_id=123&md5=11111111111111111111111111111111&organisation=Voorbeeld&archive_status=OK&pid=123&from=2017-04-06T11%3A30%3A38&until=2017-04-06T23%3A30%3A38
