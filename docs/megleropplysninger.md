---
title: "Megleropplysninger - Broker Information"
layout: default
---

<div class="language-content lang-en" markdown="1">

# Broker Information

Megleropplysninger is a product that allows brokers to request object data for delivery from the accountant. This product provides information that the accountant maintains about the property, which can be useful for the broker during the sales process.

The response structure is currently defined as a flexible record, but this may change in future versions to either predefined fields or a structured list format.

<div class="mermaid">
flowchart LR
  ambita([AMBITA]) --> infoReq["6.1.0<br/>Broker information request"]
  infoReq --> accountant([ACCOUNTANT])
  accountant --> infoRes["6.1.1<br/>Broker information response"]
  infoRes --> ambita

classDef actor fill:#ffcc00,stroke:#0a0f0f,stroke-width:1px,color:#000;
classDef request fill:#00ccff,stroke:#0a0f0f,stroke-width:1px,color:#000;
classDef response fill:#33dd33,stroke:#0a0f0f,stroke-width:1px,color:#000;
class ambita,accountant actor;
class infoReq request;
class infoRes response;
</div>

## Broker information request

On behalf of the broker the following request is made:

```json
{
  "type": "megleropplysninger",
  "ordreId": "1888e14e-1418-4d37-b3be-0d0b623681ba",
  "estateId": "8edbaf12-7e21-4cf7-8d72-74277d004c32",
  "oppdragsnummer": "8-0148/23",
  "registerenhet": {
    "type": "matrikkel",
    "ident": "3802-71-119-0-21"
  },
  "bestiller": {
    "id": "TBF",
    "navn": "Broker Doe",
    "epost": "tbf@domene.no",
    "telefon": "79119911"
  },
  "meglerkontor": {
    "orgnr": "987654323",
    "avdelingsnr": "3",
    "navn": "Avdeling3",
    "adresse": {
      "gateadresse": "Testvei 3",
      "postnummer": "0030",
      "poststed": "OSLO"
    },
    "telefon": "12345678"
  },
  "kontaktperson": {
    "id": "AO",
    "navn": "Anne Olsen",
    "epost": "aol@domene.no",
    "telefon": "12548630"
  }
}
```

### Request fields

The Megleropplysninger request uses the same basic structure as other product requests. For detailed field explanations see [common request fields](boliginformasjon.md#request-fields-that-are-in-all-requests).

The request contains only the basic required fields:
* type - Set to "megleropplysninger"
* All standard BasicProduct fields (ordreId, registerenhet, bestiller, meglerkontor, kontaktperson)

## Broker information response

The accountant responds with object data for the specified property:

```json
{
  "type": "megleropplysninger",
  "ordreId": "1888e14e-1418-4d37-b3be-0d0b623681ba",
  "objektdata": {
    "field1": "value1",
    "field2": "value2",
    "field3": 123
  },
  "forretningsforer": {
    "navn": "UNTL",
    "adresse": {
      "gateadresse": "Postboks 112 Lier",
      "postnummer": "0501",
      "poststed": "Oslo"
    },
    "epost": "post@kunde.no"
  },
  "klient": {
    "klienttype": "Borettslag tilknyttet",
    "organisasjonsnavn": "Skauen Borettslag",
    "organisasjonsnummer": "948677202",
    "epost": "styret@brl.no",
    "styreleder": {
      "navn": "Ole Styreleder",
      "epost": "leder@brl.no",
      "telefonnr": "99889988"
    }
  },
  "levert": "2022-07-07T15:48:07.6328836Z",
  "referanse": "622/1",
  "eierform": "Seksjonseier"
}
```

### Response fields

The response includes all standard callback fields plus:

* **objektdata** - Object data provided by the accountant. Currently defined as a flexible record structure with key-value pairs. **Note: This structure is preliminary and will likely change to either predefined fields or a structured list format in future versions.**
* **type** - Message type identifier ("megleropplysninger")
* **ordreId** - The order ID from the request
* **forretningsforer** (accountant) - Information about the accountant handling this property
  * navn (name) - Company name
  * adresse (address) - Address of the company
  * epost (email) - Email to the company
* **klient** - Information about the property owner organization
  * klienttype (client type) - The type of client
  * organisasjonsnavn (organization name) - The client's name
  * organisasjonsnummer (organization number) - The client's organization number
  * epost (email) - Email to the client
  * styreleder (chairperson) - The person leading the board
    * navn (name) - Chairperson's name
    * epost (email) - Chairperson's email address
    * telefonnr (phone number) - Chairperson's phone number
* **levert** (delivered) - Timestamp when the response was created
* **referanse** (reference) - Accountant's internal reference
* **eierform** (ownership type) - Type of ownership (Andelseier, Seksjonseier, Aksjonær)

When our system receives this message, it will make the broker information available to the broker system.

## Usage Notes

* This is an independent product that can be ordered at any time
* No special processing or approval workflows are involved
* The `objektdata` field structure is currently flexible and will be refined based on requirements
* For comprehensive property information, use [Boliginformasjon](boliginformasjon.md) instead

</div>

<div class="language-content lang-no" lang="no" markdown="1">

# Megleropplysninger

Megleropplysninger er et produkt som lar megler bestille objektdata til levering fra forretningsfører. Produktet gir informasjon som forretningsfører har om eiendommen, og som kan være nyttig for megler i salgsarbeidet.

Svarstrukturen er for øyeblikket definert som en fleksibel record, men dette kan endres i fremtidige versjoner til enten forhåndsdefinerte felter eller et strukturert listeformat.

<div class="mermaid">
flowchart LR
  ambita([AMBITA]) --> infoReq["6.1.0<br/>Bestilling: megleropplysninger"]
  infoReq --> accountant([FORRETNINGSFØRER])
  accountant --> infoRes["6.1.1<br/>Svar: megleropplysninger"]
  infoRes --> ambita

classDef actor fill:#ffcc00,stroke:#0a0f0f,stroke-width:1px,color:#000;
classDef request fill:#00ccff,stroke:#0a0f0f,stroke-width:1px,color:#000;
classDef response fill:#33dd33,stroke:#0a0f0f,stroke-width:1px,color:#000;
class ambita,accountant actor;
class infoReq request;
class infoRes response;
</div>

## Forespørsel om megleropplysninger

På vegne av megler sendes følgende forespørsel:

```json
{
  "type": "megleropplysninger",
  "ordreId": "1888e14e-1418-4d37-b3be-0d0b623681ba",
  "estateId": "8edbaf12-7e21-4cf7-8d72-74277d004c32",
  "oppdragsnummer": "8-0148/23",
  "registerenhet": {
    "type": "matrikkel",
    "ident": "3802-71-119-0-21"
  },
  "bestiller": {
    "id": "TBF",
    "navn": "Broker Doe",
    "epost": "tbf@domene.no",
    "telefon": "79119911"
  },
  "meglerkontor": {
    "orgnr": "987654323",
    "avdelingsnr": "3",
    "navn": "Avdeling3",
    "adresse": {
      "gateadresse": "Testvei 3",
      "postnummer": "0030",
      "poststed": "OSLO"
    },
    "telefon": "12345678"
  },
  "kontaktperson": {
    "id": "AO",
    "navn": "Anne Olsen",
    "epost": "aol@domene.no",
    "telefon": "12548630"
  }
}
```

### Felt i forespørselen

Megleropplysninger-forespørselen bruker samme grunnstruktur som andre produktmeldinger. Detaljert feltsbeskrivelse finner du under [felles felter for forespørsler](boliginformasjon.md#request-fields-that-are-in-all-requests).

Forespørselen inneholder kun de obligatoriske basisfeltene:
* type – skal settes til "megleropplysninger"
* Alle standard felter i BasicProduct (ordreId, registerenhet, bestiller, meglerkontor, kontaktperson)

## Svar med megleropplysninger

Forretningsfører svarer med objektdata for angitt eiendom:

```json
{
  "type": "megleropplysninger",
  "ordreId": "1888e14e-1418-4d37-b3be-0d0b623681ba",
  "objektdata": {
    "field1": "value1",
    "field2": "value2",
    "field3": 123
  },
  "forretningsforer": {
    "navn": "UNTL",
    "adresse": {
      "gateadresse": "Postboks 112 Lier",
      "postnummer": "0501",
      "poststed": "Oslo"
    },
    "epost": "post@kunde.no"
  },
  "klient": {
    "klienttype": "Borettslag tilknyttet",
    "organisasjonsnavn": "Skauen Borettslag",
    "organisasjonsnummer": "948677202",
    "epost": "styret@brl.no",
    "styreleder": {
      "navn": "Ole Styreleder",
      "epost": "leder@brl.no",
      "telefonnr": "99889988"
    }
  },
  "levert": "2022-07-07T15:48:07.6328836Z",
  "referanse": "622/1",
  "eierform": "Seksjonseier"
}
```

### Felt i svaret

Svaret inkluderer alle standard callback-felter og i tillegg:

* **objektdata** - Objektdata levert av forretningsfører. For øyeblikket definert som en fleksibel record-struktur med nøkkel-verdi-par. **Merk: Denne strukturen er foreløpig og vil sannsynligvis endres til enten forhåndsdefinerte felter eller et strukturert listeformat i fremtidige versjoner.**
* **type** – meldingstypen ("megleropplysninger")
* **ordreId** – ordre-ID fra forespørselen
* **forretningsforer** – informasjon om forretningsføreren som håndterer eiendommen
  * navn – selskapsnavn
  * adresse – besøks- eller postadresse
  * epost – e-postadresse
* **klient** – informasjon om eierorganisasjonen
  * klienttype – type klient
  * organisasjonsnavn – navn på klienten
  * organisasjonsnummer – organisasjonsnummer
  * epost – kontaktadresse for klienten
  * styreleder – informasjon om styreleder
    * navn – navn på styreleder
    * epost – e-postadresse til styreleder
    * telefonnr – telefonnummer til styreleder
* **levert** – tidspunkt for når svaret ble produsert
* **referanse** – intern referanse hos forretningsføreren
* **eierform** – type eierskap (andelseier, seksjonseier, aksjonær)

Når systemet vårt mottar denne meldingen, gjøres megleropplysningene tilgjengelige i meglerløsningen.

## Brukstips

* Produktet kan bestilles uavhengig av de andre meldingstypene
* Ingen spesielle prosesser eller godkjenninger er nødvendig
* Strukturen i `objektdata`-feltet er foreløpig fleksibel og vil bli raffinert basert på behov
* For mer omfattende eiendomsinformasjon bør du bruke [Boliginformasjon](boliginformasjon.md)

</div>
