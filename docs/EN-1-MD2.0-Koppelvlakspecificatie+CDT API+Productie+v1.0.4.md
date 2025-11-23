Inspections Environment and Transport Ministry of Infrastructure and Water Management

# Interface Specification Central Database Taxi Transport Notification API

Version 1.0.4

|     |     |
| --- | --- |
| Date | June 6, 2025 |
| Status | Definitive |



# COLOPHON

Location Rijnstraat 8, 2515XP The Hague Contact department connection management cdtaansluiting@ILenT.nl ILT/Information, networks and programming Inspections Environment and Transport (ILT) Ministry of Infrastructure and Water Management

- - -

Interface Specification CDT submission of supervision data - Production

# VERSION INFORMATION

| Version | Date | Particulars |
| --- | --- | --- |
| 1.0 | 20-12-2024 | Introduction of v2 of the API, initial version |
| 1.0.1 | 15-01-2025 | Rules under figure in 3.1 made consistent with API operation; inconsistencies in description of responses adjusted; For validating a driver with a foreign driving license, validation code 1 cannot be used Explicitly added that ride price excludes tip |
| 1.0.2 | 31-03-2025 | Error message OF02 added for driver number not found. |
| 1.0.3 | 15-04-2025 | Validation DF09 added for negative service duration. Validation DF10 added for end of service before end of activities. Authentication by the entrepreneur upon submission is added for service registration. |
| 1.04 | 06-06-2025 | Definitions adopted from legal CDT regulation; A few textual adjustments; 7.1.2 and 7.1.3 expanded |



Inspections Environment and Transport 1.0.4 06-06-2025

- - -

Interface Specification CDT submission of supervision data - Production

# REFERENCES

| NR  | Name | Notes |
| --- | --- | --- |
| REF-1 | Cdt-2.x.y.json | Open API specification of the REST API for CDT. NB: x and y depend on the current software version of the CDT Notifications API |
| REF-2 | DA42EN-Electronic-Driving-Licence-Reading-Manual-v2-3.pdf | Manual of the Netherlands Vehicle Authority (RDW) for the electronic reading of the Dutch driving license. |
| REF-3 | DA12 - Kentekencard Uitleesdocumentatie v2 1 1.pdf | Manual of the Netherlands Vehicle Authority (RDW) for the electronic reading of the Dutch vehicle registration certificate. |



Inspections Environment and Transport 1.0.4 06-06-2025

- - -

Interface Specification CDT submission of supervision data - Production

# TERMINOLOGY

These concepts are included to make this document independently readable. Leading for the concepts in this table are the definitions in the Decree on Passenger Transport 2000 and the Ministerial Regulation CDT.

| Definition | Description |
| --- | --- |
| Andere werkzaamheden (Other activities) | Other activities are activities performed by the driver prior to the taxi transport activities. These are reported upon registration of the service. |
| CDT-Meldingen-API (CDT Notifications API) | Facility for exchanging taxi transport data between an application and the central database taxi transport; |
| Centrale applicatie (Central application) | Application that receives taxi transport data from one or more registration tools and supplies it to the CDT via the CDT Notifications API |
| Centrale Database Taxivervoer (CDT) (Central Database Taxi Transport) | Central database of the ILT where taxi transport data is stored and processed by the ILT. The CDT is the source for the ILT regarding the taxi transport data from the driver registration tool and functionality for the entrepreneur. central |
| Chauffeur (of taxichauffeur) (Driver or taxi driver) | Natural person who performs a taxi service and in doing so drives the car used for taxi transport, acting on behalf of the entrepreneur. The term Bestuurder (Operator) is used for this in legislation and regulations. |
| Dienst (Service) | A period of working time during which the taxi driver works in the car. NB: this definition deviates from the definition in the Working Hours Act. |
| Gebeurtenis (Event) | Incident that occurs within the registration tool, being a notification, error, or malfunction. For CDT, it always concerns notifications. An event is passed on by the driver registration tool to the central application. An event is related to a service and provides clarification about the circumstances under which the taxi transport data is collected. |
| ICT-dienstverlener (ICT Service Provider) | Organization that supplies taxi transport data to the Central Database Taxi Transport on behalf of one or more taxi transporters. The ICT service provider may itself be a taxi transporter or an organization working on behalf of taxi transporters. |
| Inspectie Leefomgeving en Transport (ILT) (Inspections Environment and Transport) | National government service of the Ministry of Infrastructure and Water Management. The Inspections Environment and Transport supervises, among other things, safety and fair competition in taxi transport. It does this based on legal rules established for a driver, an entrepreneur, and the vehicle. |
| Melding (Notification) | Message sent to the CDT Notifications API |
| Ondernemer (of taxiondernemer) (Entrepreneur or taxi entrepreneur) | Authorized representative of an undertaking that performs taxi rides. The term Vervoerder (Carrier) is used for this in legislation and regulations. |
| Pauze (Break) | Break as described in Article 1:7, first paragraph, part e of the Working Hours Act. This reads: a period of at least 15 consecutive minutes, during which the work during the service is interrupted and the employee has no obligation whatsoever with regard to the agreed work. |
| Realtime (Real-time) | Direct, immediate, without delay. |
| Registratiemiddel chauffeur (Driver registration tool) | Non-standardized combination of equipment and software with which the driver immediately registers services and activities for a specific entrepreneur in a central application. The driver registration tool also registers events in a central application. |
| Rit (Ride) | Activity within a service where the driver transports passengers. |
| Taxivervoergegevens (Taxi transport data) | Data as referred to in Article 83b, second paragraph, of the Decree on Passenger Transport 2000. This data is registered by the registration tools and supplied in real-time to the CDT via the CDT Notifications API. |
| Verrichting (Activity) | Activities are actions within a service of the type ride or break. |
| Voertuig (Vehicle) | A car used for performing taxi transport. |



Inspections Environment and Transport 1.0.4 06-06-2025

- - -

Interface Specification CDT submission of supervision data - Production

# TABLE OF CONTENTS

|     |     |     |
| --- | --- | --- |
| 1   | **INTRODUCTION** | 1   |
|     | 1.1 Context | 1   |
|     | 1.2 Purpose | 1   |
| 2   | **PROCESSES** | 2   |
|     | 2.1 Register service (relate driver, entrepreneur and vehicle). | 2   |
|     | 2.2 Register ride and register break. | 2   |
|     | 2.3 Deregister ride and deregister break | 3   |
|     | 2.4 Deregister service. | 3   |
|     | 2.5 Register ICT service provider | 3   |
|     | 2.6 Deregister ICT service provider. | 3   |
|     | 2.7 Validate driver | 4   |
|     | 2.8 Requesting outstanding services and activities | 4   |
|     | 2.9 Reporting events of the driver registration tool | 4   |
|     | 2.10 Requesting driver number. | 5   |
| 3   | **MESSAGES** | 6   |
|     | 3.1 Status transitions | 6   |
|     | 3.2 Generic message data in headers | 7   |
|     | 3.3 Functional message data | 7   |
|     |     | 3.3.1 Driver |
|     |     | 3.3.2 Driving license |
|     |     | 3.3.3 Authentication. |
|     |     | 3.3.4 Entrepreneur |
|     |     | 3.3.5 Vehicle |
|     |     | 3.3.6 Other activities |
|     |     | 3.3.7 Location |
|     | 3.4 Register service (relate driver, entrepreneur and vehicle). | 11  |
|     | 3.5 Deregister service | 12  |
|     | 3.6 Register ride | 12  |
|     | 3.7 Deregister ride | 13  |
|     | 3.8 Register break | 13  |
|     | 3.9 Deregister break | 14  |
|     | 3.10 Register ICT service provider | 14  |
|     | 3.11 Deregister ICT service provider. | 15  |
|     | 3.12 Validate driver | 15  |
|     | 3.13 Requesting outstanding services and activities | 16  |
|     | 3.14 Reporting events of the driver registration tool | 17  |
|     | 3.15 Requesting driver number. | 18  |
|     | 3.16 Error message codes. | 19  |
|     |     | 3.16.1 Error messages due to headers |
|     |     | 3.16.2 Error messages due to errors in the message itself. |
|     |     | 3.16.3 Error messages due to processing content |
| 4   | **TECHNICAL REQUIREMENTS** | 27  |
|     | 4.1 Conventions | 27  |
|     |     | 4.1.1 JSON conventions |
|     |     | 4.1.2 Encoding. |
|     |     | 4.1.3 Case sensitivity. |
|     |     | 4.1.4 Date/time |
|     |     | 4.1.5 Message traffic |
|     |     | 4.1.6 Endpoints |
|     | 4.2 Data timeliness | 27  |
|     | 4.3 Availability and performance | 28  |
|     |     | 4.3.1 Availability.. |
|     |     | 4.3.2 Performance |
| 5   | **LOGGING AND CONNECTION MONITORING** | 29  |
|     | 5.1 Logging | 29  |
|     | 5.2 Connection monitoring | 29  |
| 6   | **ERROR HANDLING** | 30  |
|     | 6.1 General. | 30  |
|     | 6.2 Error in calling service-related endpoints. | 30  |
|     | 6.3 Error in calling /verbinding | 30  |
|     | 6.4 Duplicate detection | 30  |
| 7   | **AUTHENTICATION AND INFORMATION SECURITY** | 31  |
|     | 7.1 Authentication | 31  |
|     |     | 7.1.1 PKI Certificates |
|     |     | 7.1.2 Driving license authentication |
|     |     | 7.1.3 Vehicle registration certificate authentication |
|     | 7.2 Information security. | 31  |
|     |     | 7.2.1 Transport Layer Security (TLS) |
|     |     | 7.2.2 API-keys |
|     | 7.3 Headers. | 33  |



Inspections Environment and Transport 1.0.4 06-06-2025

- - -

# 1 INTRODUCTION

This interface specification describes the following functions of the CDT Notifications API:

1.  The submission of services, rides, and breaks by the entrepreneur;

2.  The **validation of driver data**;

3.  The **requesting of outstanding services and activities** within the CDT;

4.  The **registration and deregistration of ICT service providers** by entrepreneurs;

5.  The **reporting of events** of the driver registration tool related to a service.

6.  The **requesting of the driver number** of a taxi driver based on the number of their Dutch driving license.


## 1.1 Context

The driver uses a **Driver Registration Tool** to register **real-time** relevant information about the execution of taxi transport for the entrepreneur in the **central application**, which in turn supplies the data in real-time to the **CDT**. For the exchange of messages with the ILT, the **CDT Notifications API** (based on REST) has been developed, which must be called by the Central Application.

## 1.2 Purpose

This document is an appendix to the CDT Regulation. The purpose of this document is to provide insight into the functions and operation of the CDT Notifications API to parties wishing to use it.

- - -

# 2 PROCESSES

In this interface specification, the term "**dienst**" (service) is used as it is in everyday language. The meaning of service in this document is therefore not the definition of Service in Article 1.7, first paragraph, part c of the Working Hours Act. In this interface specification, "**dienst**" refers to "**the activities on board the car used for taxi transport**".

The following processes are in scope for the submission to the CDT:

1.  Register service (relate driver, entrepreneur, and vehicle);

2.  Report other activities of the driver preceding the service;

3.  Register ride or break;

4.  Deregister ride or break;

5.  Deregister service;

6.  Register ICT service provider by entrepreneur;

7.  Deregister ICT service provider by entrepreneur;

8.  Validate driver;

9.  Request outstanding services and activities;

10.  Report events of the driver registration tool;

11.  Request driver number.


## 2.1 Register service (relate driver, entrepreneur and vehicle)

**Precondition** The entrepreneur has ensured that:

1.  The ICT service provider is registered with the CDT Notifications API;

2.  The vehicle is validated;

3.  The driver is validated;

4.  The driver has access to the 'driver registration tool'.


**Process** The driver:

1.  Authenticates using an allowed authentication method;

2.  Optionally enters start and end times of other activities performed prior to the service;

3.  Registers the service, including the entrepreneur and the vehicle, with the central application via the driver registration tool, which then **immediately** registers the service with the CDT Notifications API.


**Postcondition** The registration of the service is recorded for the driver, the vehicle, and the entrepreneur in the central application and in the CDT. The optional registration of other activities is recorded for the driver.

## 2.2 Register ride and register break

**Precondition**

*   The service within which the ride or break takes place is registered in the central application and the CDT;

*   For break registration: there are no non-deregistered rides or breaks within this service;

*   For ride registration: there are no non-deregistered breaks within this service;


**Process**

*   The driver registers the ride or break with the central application via the driver registration tool, which then **immediately** registers the ride or break with the CDT Notifications API. The **location is mandatory** when registering a ride. If no current location is available, the last known location is provided.


**Postcondition** The registration of the ride or break is recorded within the driver's service in the central application and the CDT.

## 2.3 Deregister ride and deregister break

**Precondition** The ride or break is registered in the central application and the CDT.

**Process** The driver deregisters the ride or break with the central application via the driver registration tool, which then **immediately** deregisters the ride or break with the CDT Notifications API.

**Postcondition** The deregistration of the ride or break is recorded within the service for the driver and the entrepreneur in the central application and the CDT.

## 2.4 Deregister service

**Precondition** The service is registered and all rides and breaks within the service are deregistered in the central application and the CDT.

**Process** The driver deregisters the service with the central application via the driver registration tool, which then **immediately** deregisters the service with the CDT Notifications API.

**Postcondition** The deregistration of the service is recorded in the central application and the CDT.

## 2.5 Register ICT service provider

**Precondition** The entrepreneur purchases an ICT solution from the ICT service provider and has not registered the ICT service provider with the CDT Notifications API.

**Process** The central application sends the entrepreneur's registration for the ICT service provider to the CDT Notifications API.

**Postcondition** The ICT service provider is registered by the entrepreneur with the CDT Notifications API.

## 2.6 Deregister ICT service provider

**Precondition** The entrepreneur has stopped purchasing the ICT solution from the ICT service provider and; The ICT service provider is registered by the entrepreneur with the CDT Notifications API.

**Process** The central application sends the entrepreneur's deregistration for the ICT service provider to the CDT Notifications API.

**Postcondition** The ICT service provider is deregistered by the entrepreneur with the CDT Notifications API.

## 2.7 Validate driver

**Precondition** Entrepreneur does not have the information whether a driver is authorized.

**Process** Entrepreneur validates the driver data with the CDT via the Central application. **NB: only with validation code 0 will the driver data be considered validated.**

**Postcondition** Entrepreneur has information whether the provided driver data belongs to an authorized driver.

## 2.8 Requesting outstanding services and activities

**Precondition** Central application does not have the information about which services and activities have been outstanding in the CDT for longer than a specified duration.

**Process** Central application requests services and activities that have been outstanding for longer than a specified duration (**minimum 24 hours**) from the CDT.

**Postcondition** Central application has information regarding which services and activities have been outstanding in the CDT for longer than the specified duration.

## 2.9 Reporting events of the driver registration tool

Events of the driver registration tool are notifications (M). A further explanation is provided in paragraph 3.14 of this document.

**Precondition** The service to which the event is related is registered in the central application and the CDT.

**Process** The driver registration tool reports the event to the central application, which then **immediately** reports the event to the CDT Notifications API.

**Postcondition** The event is recorded in the central application and the CDT.

## 2.10 Requesting driver number

The driver number is not listed on the decision for driver cards issued before 2025. To find out the driver number, the driver number can be requested from the CDT Notifications API based on the data of a **Dutch driving license**.

**Precondition** Driver and entrepreneur do not have the driver number and the driver has a valid Dutch driving license and their BSN is registered with Kiwa.

**Process** Entrepreneur requests the driver number from the CDT based on the driver's Dutch driving license.

**Postcondition** If the driver has a valid taxi authorization, the CDT returns the driver number to the entrepreneur. **NB:** for drivers who do not have a Dutch driving license or who are not registered with a BSN at KIWA, it is not possible to request the driver number based on driving license data. These drivers will receive the driver number by letter from KIWA and must pass the driver number on to the entrepreneur.

- - -

# 3 MESSAGES

Messages consist of **generic message data** (metadata) and the content of the message itself. ILT has chosen to include the metadata as **headers** in the message.

## 3.1 Status transitions

The logical coherence between the messages and the status of a service and activity is shown in the figure below. The messages "request outstanding services" and "validate drivers" do not appear in the figure below because they do not affect the status of services and activities.

**Rules:**

*   Services may only be registered by a **registered ICT service provider**;

*   The registration of an activity (ride or break) requires a **registered service**;

*   The registration of an event requires a **registered service**;

*   To deregister a service, **all activities** within that service must be deregistered;

*   Events can be reported during a service, regardless of whether an activity is taking place;

*   The following rules apply to overlapping activities:

    *   Overlapping rides are allowed;

    *   A break **cannot** be registered during a ride;

    *   A ride **cannot** be registered during a break...

*   A break can only be reported with a registration time that is **after the last deregistered activity**.


## 3.2 Generic message data in headers

The fields in the table below are in the message header.

| Field name | Type/length | Example | Explanation |
| --- | --- | --- | --- |
| **Dienstverlener** (Service Provider) | UUID | `a0eebc99-9c0b-4ef8-bb6d-6bb9bd380a11` | Key issued by ILT that uniquely identifies the submitting ICT service provider to the CDT Notifications API. |
| **ext\_key** | UUID | `a0eebc99-9c0b-4ef8-bb6d-6bb9bd380a11` | Unique key for access to the API gateway |
| **Bericht-Id** (Message ID) | UUID | `a0eebc99-9c0b-4ef8-bb6d-6bb9bd380a11` | Identification of a message. **NB: this must be generated when sending the message.** |
| **Verzendtijdstip** (Send Time) | `date-time` in UTC conforming to IETF RFC3339 | `2023-03-31T14:55:44Z` or `2023-03-31T14:55:44.444Z` | Send time of message from ICT service provider to ILT. **NB: this time must be updated with every sending attempt.** |
| **Softwareversie-Registratiemiddel** (Software Version Registration Tool) | String(20) | `v12.23.124` | Software version of the registration tool. |
| **Softwareversie-Centrale-Applicatie** (Software Version Central Application) | String (20) | `v2.2.9` | Software version of the Central application |



_1: Softwareversie-Registratiemiddel may be an empty string if the message does not originate from a "driver registration tool"._

## 3.3 Functional message data

The fields in the table below may appear in a message (in the message body); it is described per message which of these fields appear. In addition, paragraphs 3.3.1 to 3.3.7 contain objects that may appear in messages.

| Field name | Type/length | Examples | Explanation |
| --- | --- | --- | --- |
| **id** | UUID | `a0eebc99-9c0b-4ef8-bb6d-6bb9bd380a11` | Identification of a service, activity, or event **NB: this must be generated when the service, activity, or event occurs.** |
| **aanmeldtijdstip** (registration time) | `date-time` in UTC conforming to IETF RFC3339 | `2023-03-31T14:55:44Z` or `2023-03-31T14:55:44.444Z` | The time at which the service or activity started. **NB: this is the time on the registration tool.** |
| **afmeldtijdstip** (deregistration time) | `date-time` in UTC conforming to IETF RFC3339 | `2023-03-31T14:55:44Z` or `2023-03-31T14:55:44.444Z` | The time at which the service or activity ended. **NB: this is the time on the registration tool.** |
| **ritprijs** (ride price) | Integer(6) | `10170` | Total final price of the completed ride in euro cents. **This excludes tip.** |
| **afstand** (distance) | Numeric (4,1) | `12,1` | Distance covered during the ride |
| **registratietijdstip** (recording time) | `date-time` in UTC conforming to IETF RFC3339 | `2023-03-31T14:55:44Z` or `2023-03-31T14:55:44.444Z` | The time at which registration takes place on the registration tool. |
| **gebeurtenistijdstip** (event time) | `date-time` in UTC conforming to IETF RFC3339 | `2023-03-31T14:55:44Z` or `2023-03-31T14:55:44.444Z` | Time at which the event occurs on the registration tool. |
| **gebeurteniscode** (event code) | String(4) | `M106` | code from table in paragraph 3.14 |



**Rules:**

*   It is indicated in the message which fields are mandatory;

*   Message is rejected if fields do not have the correct format or layout;

*   Message is rejected if mandatory fields are missing;

*   Message is rejected if fields other than those permitted are present;

*   Message is rejected if permitted fields occur more often than specified.


In addition to the fields in the table above, objects can also be sent. These are described in the following paragraphs.

### 3.3.1 Chauffeur (Driver)

| components | type | RegEx | example | Explanation |
| --- | --- | --- | --- | --- |
| **chauffeursnummer** (driver number) | String(8) | `^T\\d{7}$` | `T0012345` | Driver number as issued by KIWA |
| **gevalideerd** (validated) | boolean |     | `true` | indication whether the driving license has been validated at the CDT Notifications API. This may only be set to `true` if validation code **0** was returned for the driver/entrepreneur combination upon calling `POST https://[host]/v2/chauffeurs/valideren`. |
| **rijbewijs** (driving license) | object |     |     | see paragraph 3.3.2 |



**Example:**

JSON

```
"chauffeur": {
  "chauffeursnummer": "T0012345",
  "gevalideerd": true,
  "rijbewijs": {
    "land": "NL",
    "nummer": "123456789"
  }
}
```

### 3.3.2 Rijbewijs (Driving license)

| components | type | RegEx | example | Explanation |
| --- | --- | --- | --- | --- |
| **land** (country) | string(2) | `^[A-Z]{2}$` | `NL` | Country code according to **ISO3166-1 alpha-2** |
| **rijbewijsnummer** (driving license number) | string(16) | `^[0-9a-zA-Z]{1,16}$` | `1234567890` | A Dutch driving license number consists of 10 digits, other countries may differ. Hyphens, spaces, and dots are omitted. |



**Example:**

JSON

```
"rijbewijs": {
  "land": "NL",
  "rijbewijsnummer": "123456789"
}
```

### 3.3.3 Authenticatie (Authentication)

| components | type | RegEx | example | Explanation |
| --- | --- | --- | --- | --- |
| **middel** (means) | string{4} | `RBNL\|BIO\|2FA\|geen` | RBNL | RBNL = Dutch driving license<br><br>  <br><br>BIO = Biometrics<br><br>  <br><br>2FA = 2-factor authentication<br><br>  <br><br>geen = driver is not authenticated (NB: only to be used for service registration by carrier for retrospective submission). |
| **kenmerk** (characteristic) | string(32) |     | `1234565677` `vingerafdruk` `gezichtsherkenning` `wachtwoord-SMS` `pincode-koppelcode` `geen` | For RBNL → driving license number.<br><br>  <br><br>For BIO → fingerprint, facial recognition.<br><br>  <br><br>For 2FA → password-SMS, password-link code, PIN code-SMS, PIN code-link code.<br><br>  <br><br>For geen → none. |



**Examples:**

JSON

```
"authenticatie": {
  "middel": "RBNL",
  "kenmerk": "2090710264"
}
"authenticatie": {
  "middel": "BIO",
  "kenmerk": "gezichtsherkenning"
}
"authenticatie": {
  "middel": "2FA",
  "kenmerk": "wachtwoord-SMS"
}
"authenticatie": {
  "middel": "geen",
  "kenmerk": "geen"
}
```

### 3.3.4 Ondernemer (Entrepreneur)

| components | type | RegEx | example | Explanation |
| --- | --- | --- | --- | --- |
| **kiwaNummer** (kiwa Number) | string(7) | `^P\\d{4,6}$` | `P123456` | KIWA taxi permit number |
| **kvkNummer** (kvk Number) | string(8) | `^\\d{8}$` | `12345678` | Chamber of Commerce number |



**Example:**

JSON

```
"ondernemer": {
  "kvkNummer": "12345678",
  "kiwaNummer": "P1234"
}
```

### 3.3.5 Voertuig (Vehicle)

| components | type | RegEx | example | Explanation |
| --- | --- | --- | --- | --- |
| **kenteken** (license plate) | string(6) | `^[0-9A-Z]{6}$` | `P390HV` | license plate of the vehicle |
| **validatiemethode** (validation method) | string (1) | `K\|N` | `K` | "K": license plate card read<br><br>  <br><br>"N": not validated |
| **validatiedatum** (validation date) | string(10) | `^\d{4}-\d{2}-\d{2}$` | `2024-03-04` | date on which validation was performed, format **YYYY-MM-DD**. If `validatiemethode = "N"`, use the current date. |



**Example:**

JSON

```
"voertuig": {
  "kenteken": "P390HV",
  "validatiemethode": "N",
  "validatiedatum": "2024-03-04"
}
```

### 3.3.6 Andere werkzaamheden (Other activities)

| components | type | example | Explanation |
| --- | --- | --- | --- |
| **begintijdstip** (start time) | `date-time` in UTC conforming to IETF RFC3339 | `2023-03-31T14:55:44.444Z` | date and time at which other activities started |
| **eindetijdstip** (end time) | `date-time` in UTC conforming to IETF RFC3339 | `2023-03-31T14:55:44.444Z` | date and time at which other activities ended |



**Example:**

JSON

```
"andere Werkzaamheden": [
  {
    "begintijdstip": "2023-03-31T14:44:55.000Z",
    "eindetijdstip": "2023-03-31T15:44:55.000Z"
  },
  {
    "begintijdstip": "2023-03-31T18:44:55.000Z",
    "eindetijdstip": "2023-03-31T19:44:55.000Z"
  }
]
```

### 3.3.7 Locatie (Location)

| components | type | RegEx | example | Explanation |
| --- | --- | --- | --- | --- |
| **breedtegraad** (latitude) | string | `^\[-+\]?(90(.0{4,6})? \| (\[1-\]?\\d(.\\d{4,6})?))$` | `5.100056` | geographical latitude with 4 to 6 digits after the decimal point |
| **lengtegraad** (longitude) | string | `^\[-+\]?(180(.0{4,6})? \| ((1\[0-7\] \| \[1-9\])?\\d(.\\d{4,6})?))$` | `52.086491` | geographical longitude with 4 to 6 digits after the decimal point |



**Example:**

JSON

```
"locatie": {
  "breedtegraad": "5.10005",
  "lengtegraad": "52.08649"
}
```

## 3.4 Aanmelden dienst (relateren chauffeur, ondernemer en voertuig) (Register service (relate driver, entrepreneur and vehicle))

**Use cases:** Register service at the start of activities on board the car used for taxi transport, possibly with preceding other activities.

**Endpoint:** `POST https://[host]/v2/diensten`

| Field name | Mandatory |
| --- | --- |
| **id** (of the service) | yes |
| **chauffeur** (driver) | yes |
| **authenticatie** (authentication) | yes |
| **ondernemer** (entrepreneur) | yes |
| **voertuig** (vehicle) | yes |
| **aanmeldtijdstip** (registration time) | yes |
| **registratietijdstip** (recording time) | yes |
| **andere Werkzaamheden** (other activities) | no  |



**Response sunny day: '201 CREATED'**

Fields in the response body:

*   `data`

*   `id`


Notifications in the table below are possible with a '201 CREATED' result code.

| Status code | Notification code | Notification text | Remark |
| --- | --- | --- | --- |
| 201 | **DF00** | There are<br><br>#<br><br>services active for the driver with the ICT service provider. | There is a non-closed service registered with ILT for the specified driver with this ICT service provider. |
| 201 | **DF06** | 'voertuig.kenteken' is in use on another service with the ICT service provider. | There is a non-closed service registered with ILT for the specified vehicle with the ICT service provider. |
| 201 | **DF07** | 'chauffeur' is not validated while it is marked as validated. | A driver may only be reported as validated if a successful validation has been performed. |
| 201 | **DF08** | 'ICT-dienstverlener' is not registered for entrepreneur | An entrepreneur must register which ICT service provider is supplying for them. |



**Response for outstanding services: '201 CREATED'**

Data of outstanding service(s) with the submitting ICT service provider is in the message body.

Fields in the response body:

JSON

```
data: {
  id: "...",
  meldingen: [
    {
      code: "...",
      tekst: "...",
      details: [
        {
          openstaande Diensten: [
            {
              id: "...",
              aanmeldtijdstip: "...",
              openstaande Verrichtingen: [
                {
                  id: "...",
                  aanmeldtijdstip: "..."
                }
              ]
            }
          ]
        }
      ]
    }
  ]
}
```

## 3.5 Afmelden dienst (Deregister service)

**Use cases:** Deregister service at the end of activities on board the car used for taxi transport.

**Endpoint:** `POST https://[host]/v2/diensten/{dienst.id}/afmelden`

| Field name | Mandatory |
| --- | --- |
| **afmeldtijdstip** (deregistration time) | yes |
| **registratietijdstip** (recording time) | yes |



**Response sunny day: '200 OK'**

Fields in the response body:

*   `data`

*   `id`


If error code **DF05** is returned, the response is: `'400'`.

JSON

```
data: {
  foutmelding: "bericht afgekeurd",
  aantal: "...",
  fouten: [
    {
      code: "...",
      tekst: "...",
      details: {
        openstaande Verrichtingen: [
          {
            id: "...",
            aanmeldtijdstip: "..."
          }
        ]
      }
    }
  ]
}
```

## 3.6 Aanmelden rit (Register ride)

**Use cases:** Register ride when (first) passenger gets in.

**Endpoint:** `POST https://[host]/v2/diensten/{dienst.id}/ritten`

| Field name | Mandatory |
| --- | --- |
| **id** (of the ride) | yes |
| **aanmeldtijdstip** (registration time) | yes |
| **registratietijdstip** (recording time) | yes |
| **locatie** (location) | yes |



**Response sunny day: '201 CREATED'**

Fields in the response body:

*   `data`

*   `id`


## 3.7 Afmelden rit (Deregister ride)

**Use cases:** Deregister ride when (last) passenger gets out.

**Endpoint:** `POST https://[host]/v2/diensten/{dienst.id}/ritten/{rit.id}/afmelden`

| Field | Mandatory |
| --- | --- |
| **afmeldtijdstip** (deregistration time) | yes |
| **registratietijdstip** (recording time) | yes |
| **afstand** (distance) | yes |
| **ritprijs** (ride price) | yes |



**NB.** For contract transport and group transport, the price is not mandatory. In that case, `ritprijs` may be **0 (zero)**.

**Response sunny day: '200 OK'**

Fields in the response body:

*   `data`

*   `id`


## 3.8 Aanmelden pauze (Register break)

**Use cases:** Register break at the start of the break;

**Endpoint:** `POST https://[host]/v2/diensten/{dienst.id}/pauzes`

| Field name | Mandatory |
| --- | --- |
| **id** (of the break) | yes |
| **aanmeldtijdstip** (registration time) | yes |
| **registratietijdstip** (recording time) | yes |



**Response sunny day: '201 CREATED'**

Fields in the response body:

*   `data`

*   `id`


## 3.9 Afmelden pauze (Deregister break)

**Use cases:** Deregister break at the end of the break;

**Endpoint:** `POST https://[host]/v2/diensten/{dienst.id}/pauzes/{pauze.id}/afmelden`

| Field | Mandatory |
| --- | --- |
| **afmeldtijdstip** (deregistration time) | yes |
| **registratietijdstip** (recording time) | yes |



**Response sunny day: '200 OK'**

Fields in the response body:

*   `data`

*   `id`


## 3.10 Aanmelden ICT-dienstverlener (Register ICT service provider)

**Use case:** entrepreneur registers for the use of the ICT solution of the ICT service provider.

**Endpoint:** `POST https://[host]/v2/ondernemers/aanmelden`

| Field | Mandatory |
| --- | --- |
| **ondernemer** (entrepreneur) | yes |



**Response sunny day: '200 OK'**

Fields in the response body:

JSON

```
data: {
  validaties: [
    {
      validatiecode: "...",
      verificatie-omschrijving: "..."
    }
  ]
}
```

| Validation code | Verification description |
| --- | --- |
| **0** | 'ondernemer.kiwaNummer' is from a licensed taxi entrepreneur; 'ondernemer.kvkNummer' is from an existing business |
| **1** | 'ondernemer.kiwaNummer' is unknown |
| **2** | 'ondernemer.kiwaNummer' is not from a licensed taxi entrepreneur |
| **3** | 'ondernemer.kvkNummer' is unknown |
| **4** | 'ondernemer.kvkNummer' is from a non-active business |



## 3.11 Afmelden ICT-dienstverlener (Deregister ICT service provider)

**Use case:** entrepreneur stops using the ICT solution of the ICT service provider.

**Endpoint:** `POST https://[host]/v2/ondernemers/{ondernemer.kiwaNummer}/afmelden`

**Response sunny day: '200 OK'**

There is no message body present. If the entrepreneur is unknown to the ICT service provider, a '404 - not found' response is given.

## 3.12 Valideren chauffeur (Validate driver)

**Use cases:**

*   Check whether a driver's Dutch driving license number and driver number are valid and belong to the same person.

*   Check whether the driver number of a driver with a foreign driving license exists and is valid.


**Endpoint:** `POST https://[host]/v2/chauffeurs/valideren`

| Field | Mandatory |
| --- | --- |
| **chauffeur** (driver) | yes |
| **ondernemer** (entrepreneur) | yes |



**NB:** the driver object has the component "**gevalideerd**" (validated). The value of this must be `false` for this query.

**Response sunny day: '200 OK'**

Fields in the response body:

JSON

```
data: {
  validaties: [
    {
      validatiecode: "...",
      validatieomschrijving: "..."
    }
  ]
}
```

| Validation code | description for Dutch driving license | description for foreign driving license |
| --- | --- | --- |
| **0** | 'chauffeur.chauffeursnummer' is from an authorized driver; 'chauffeur.rijbewijs.rijbewijsnummer' is from a valid driving license and both belong to the same driver | 'chauffeur.chauffeursnummer' is from an authorized driver |
| **1** | 'chauffeur.chauffeursnummer' is from a different driver than 'chauffeur.rijbewijs.rijbewijsnummer' | not applicable |
| **2** | 'chauffeur.chauffeursnummer' unknown | 'chauffeur.chauffeursnummer' unknown |
| **3** | 'chauffeur.rijbewijs.rijbewijsnummer' unknown | not applicable |
| **4** | 'chauffeur.chauffeursnummer' is from an unauthorized driver | 'chauffeur.chauffeursnummer' is from an unauthorized driver |
| **5** | driving license is invalid | not applicable |



## 3.13 Opvragen van openstaande diensten en verrichtingen (Requesting outstanding services and activities)

**Use case:** Request outstanding services and activities.

**Endpoint:** `GET https://[host]/v2/diensten/openstaand?ouderdan=24`

Returns all non-deregistered services with possibly non-deregistered activities with a registration time older than `x` hours ago for ICT service provider Y, where `x` has a default value of **24**. A value lower than 24 is ignored.

**Response sunny day: '200 OK'**

Fields in the response body:

JSON

```
data: {
  openstaande Diensten: [
    {
      id: "...",
      aanmeldtijdstip: "...",
      openstaande Verrichtingen: [
        {
          id: "...",
          aanmeldtijdstip: "..."
        }
      ]
    }
  ]
}
```

If there are no outstanding services, the call receives a **'204 No Content'** response without a message body.

## 3.14 Melden van gebeurtenissen van het registratiemiddel chauffeur (Reporting events of the driver registration tool)

**Use case:** Report an event on the driver registration tool to the ILT.

**Endpoint:** `POST https://[host]/v2/diensten/{dienstId}/gebeurtenissen`

| Field | Mandatory |
| --- | --- |
| **id** (of the event) | Yes |
| **gebeurtenistijdstip** (event time) | Yes |
| **registratietijdstip** (recording time) | Yes |
| **gebeurteniscode** (event code) | Yes |
| **authenticatie** (authentication) | No \*1 |
| **locatie** (location) | No \*1 |



_1: see table below when this field is mandatory._

The codes for Notifications are mandatory.

| Event code | Description | Field mandatory |
| --- | --- | --- |
| **M100** | Authentication attempt before and during service unsuccessful. | `authenticatie` |
| **M101** | Failure of driver registration tool during service. |     |
| **M102** | No position determination for longer than 3 minutes. | `locatie` (last known location) |
| **M103** | Position determination successful after event M102. | `locatie` |
| **M104** | Time on registration tool not synchronized for longer than 10 minutes. |     |
| **M105** | Time on registration tool synchronized after reporting synchronization problem. |     |
| **M106** | No movement detected for longer than 1 minute during 'ride' activity while position determination suggests movement. |     |
| **M107** | Movement detected while position determination suggests movement after notification M106. |     |
| **M108** | No data connection for longer than 1 minute with driver registration tool. |     |
| **M109** | Data connection restored with driver registration tool after interruption. |     |
| **M110** | Service deregistered by entrepreneur |     |
| **M111** | Activity deregistered by entrepreneur |     |
| **M112** | Accidentally started service/activity closed. |     |
| **M113** | Ride price has been manually entered. |     |



**Response sunny day: '201 OK'**

Fields in the response body:

*   `data`

*   `id`


## 3.15 Opvragen chauffeursnummer (Requesting driver number)

**Use case:** Requesting a driver number based on a Dutch driving license number.

**Endpoint:** `POST https://[host]/v2/chauffeursnummer/opvragen`

| Field | Mandatory |
| --- | --- |
| **rijbewijs** (driving license) | Yes |



**Response if found: '200 OK'**

Fields in the response body:

*   `data`

*   `chauffeursnummer`


## 3.16 Foutmeldingscodes (Error message codes)

Errors are returned in the response body in the following format:

JSON

```
data: {
  foutmelding: "...",
  aantal: "...",
  fouten: [
    {
      code: "...",
      tekst: "...",
      details: {
        openstaande Verrichtingen: [
          {
            id: "...",
            aanmeldtijdstip: "..."
          }
        ]
      }
    }
  ]
}
```

| Code prefix | Type of error |
| --- | --- |
| **Hxxx** | technical error in (one of the) header(s). |
| **HFxx** | functional error in (one of the) header(s). |
| **Gxxx** | generic (technical) error. |
| **DFxx** | functional error in service message. |
| **VFxx** | functional error in activity message. |
| **BFxx** | functional error in event message. |
| **OFxx** | functional error in query |



### 3.16.1 Foutmeldingen wegens headers (Error messages due to headers)

The following messages can be returned on all calls:

| Status code | Notification code | Notification text | Explanation |
| --- | --- | --- | --- |
| 400 | **H000** | Missing header \<headernaam\>. | Dienstverlener, BerichtId, Softwareversie-Registratiemiddel, Softwareversie-Centrale-Applicatie or Verzendtijdstip is missing. |
| 400 | **H001** | Value of 'Bericht-Id' does not comply with the format. | Must be UUID |
| 400 | **H002** | Value of 'Verzendtijdstip' does not comply with the format. | IETF RFC3339 "date-time" specification. |
| 400 | **H003** | Value of 'Verzendtijdstip' is in the future. | A message cannot have been sent in the future. |
| 400 | **H004** | Value of 'Softwareversie-Registratiemiddel' does not comply with the format. | `^[0-9A-Za-z.-]{2,20}$`, may be an empty string in specific cases. |
| 400 | **H005** | Value of 'Softwareversie-Centrale-Applicatie' does not comply with the format. | `^[0-9A-Za-z.-]{2,20}$` |
| 400 | **H006** | Value of 'Dienstverlener' does not comply with the format. | Must be UUID |
| 400 | **HF00** | Unknown Dienstverlener. | Dienstverlener code is not active or unknown. |
| 400 | **HF10** | Bericht-Id is not unique. | The BerichtId in the header must be unique |

Note: Due to the way the validations are performed, header errors will only occur on messages that do not produce other errors.


### Code Mapping of API Calls for Error Messages


| Code | API Call NL (Aanroep)        | API Call                     |
| ---- | ---------------------------- | ---------------------------- |
| A    | Aanmelden dienst             | Register service             |
| B    | Afmelden dienst              | Deregister service           |
| C    | Aanmelden rit                | Register ride                |
| D    | Afmelden rit                 | Deregister ride              |
| E    | Aanmelden pauze              | Register break               |
| F    | Afmelden pauze               | Deregister break             |
| G    | Aanmelden ICT-dienstverlener | Register IT service provider |
| H    | Valideren chauffeur          | Validate driver              |
| I    | Melden gebeurtenissen        | Report events                |
| J    | Opvragen chauffeursnummer    | Request driver number        |

Note: 'Request open services' and 'deregister IT service provider' are not included because they contain no message body.


### 3.16.2 Foutmeldingen wegens fouten in het bericht zelf (Error messages due to errors in the message itself)


| Status | Code | Text                                                                                                                            | Explanation                       | API Call Codes      |
| ------ | ---- | ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------- | ------------------- |
| 400    | G000 | Invalid JSON.                                                                                                                   | Invalid JSON formatting.          | A,B,C,D,E,F,G,H,I,J |
| 400    | G001 | Double field.                                                                                                                   | Data must be unique.              | A,B,C,D,E,F,G,H,I,J |
| 400    | G010 | 'aanmeldtijdstip' (registration time) is missing.                                                                               | Required field.                   | A,C,E               |
| 400    | G011 | Value of 'aanmeldtijdstip' (registration time) does not conform to the format.                                                  | ISO UTC yyyy-MM-ddThh:mm:ss.sssZ. | A,C,E               |
| 400    | G012 | Value of 'aanmeldtijdstip' (registration time) is in the future.                                                                |                                   | A,C,E               |
| 400    | G020 | 'registratietijdstip' (registration time) is missing.                                                                           |                                   | A,B,C,D,E,F,I       |
| 400    | G021 | Value of 'registratietijdstip' (registration time) does not conform to the format.                                              |                                   | A,B,C,D,E,F,I       |
| 400    | G022 | Value of 'registratietijdstip' (registration time) is in the future.                                                            |                                   | A,B,C,D,E,F,I       |
| 400    | G030 | 'afmeldtijdstip' (check-out time) is missing.                                                                                   |                                   | B,D,F               |
| 400    | G031 | Value of 'afmeldtijdstip' (check-out time) does not conform to the format.                                                      |                                   | B,D,F               |
| 400    | G032 | Value of 'afmeldtijdstip' (check-out time) is in the future.                                                                    |                                   | B,D,F               |
| 400    | G040 | 'id' (ID) is missing.                                                                                                           | Required.                         | A,C,E,I             |
| 400    | G041 | Value of 'id' (ID) does not conform to the format.                                                                              | UUID.                             | A,C,E,I             |
| 400    | G050 | Value of path parameter 'dienst' (employ) does not conform to the format.                                                       | UUID.                             | B,C,D,E,F,I         |
| 400    | G060 | 'chauffeur' (driver) is missing.                                                                                                |                                   | A,H                 |
| 400    | G061 | 'chauffeur.chauffeursnummer' (driver.driver number) is missing.                                                                 |                                   | A,H                 |
| 400    | G062 | Value of 'chauffeur.chauffeursnummer' (driver.driver number) does not conform to the format.                                    |                                   | A,H                 |
| 400    | G063 | 'chauffeur.gevalideerd' (driver validated) is missing.                                                                          |                                   | A                   |
| 400    | G064 | Value of 'chauffeur.gevalideerd' (driver validated) does not conform to the format.                                             | Boolean.                          | A                   |
| 400    | G070 | 'chauffeur.rijbewijs' (driver's license) is missing.                                                                            |                                   | A,H,J               |
| 400    | G071 | 'chauffeur.rijbewijs.rijbewijsnummer' (driver's license.driver's license number) is missing.                                    |                                   | A,H,J               |
| 400    | G072 | Value of 'chauffeur.rijbewijs.rijbewijsnummer' (driver's license.driver's license number) does not conform to the format.       |                                   | A,H,J               |
| 400    | G073 | 'chauffeur.rijbewijs.land' (driver.license.country) is missing.                                                                 |                                   | A,H,J               |
| 400    | G074 | Value of 'chauffeur.rijbewijs.land' (driver.license.country) does not conform to the format.                                    |                                   | A,H,J               |
| 400    | G080 | 'authenticatie' (authentication) is missing.                                                                                    |                                   | A,I                 |
| 400    | G081 | 'authenticatie.middel' (authentication.means) is missing.                                                                       |                                   | A,I                 |
| 400    | G082 | Value of 'authenticatie.middel' (authentication.means) does not conform to the format.                                          |                                   | A,I                 |
| 400    | G083 | 'authenticatie.kenmerk' (authentication feature) is missing.                                                                    |                                   | A,I                 |
| 400    | G084 | Value of 'authenticatie.kenmerk' (authentication feature) does not conform to the format.                                       |                                   | A,I                 |
| 400    | G090 | 'ondernemer' (entrepreneur) is missing.                                                                                         |                                   | A,G,H               |
| 400    | G091 | 'ondernemer.kiwaNummer' (entrepreneur.kiwaNumber) is missing.                                                                   |                                   | A,G,H               |
| 400    | G092 | Value of 'ondernemer.kiwaNummer' (entrepreneur.kiwaNumber) does not conform to the format.                                      |                                   | A,G,H               |
| 400    | G093 | 'ondernemer.kvkNummer' (entrepreneur.Chamber of Commerce number) is missing.                                                    |                                   | A,G,H               |
| 400    | G094 | Value of 'ondernemer.kvkNummer' (entrepreneur.Chamber of Commerce number) does not conform to the format.                       |                                   | A,G,H               |
| 400    | G100 | 'voertuig' (vehicle) is missing.                                                                                                |                                   | A                   |
| 400    | G101 | 'voertuig.kenteken' (vehicle registration number) is missing.                                                                   |                                   | A                   |
| 400    | G103 | Value of 'voertuig.kenteken' (vehicle registration number) does not conform to the format.                                      |                                   | A                   |
| 400    | G104 | 'voertuig.validatiemethode' (vehicle validation method) is missing.                                                             |                                   | A                   |
| 400    | G105 | Value of 'voertuig.validatiemethode' (vehicle validation method) does not conform to the format.                                |                                   | A                   |
| 400    | G106 | 'voertuig.validatiedatum' (vehicle validation date) is missing.                                                                 |                                   | A                   |
| 400    | G107 | Value of 'voertuig.validatiedatum' (vehicle validation date) does not conform to the format.                                    |                                   | A                   |
| 400    | G108 | Value of 'voertuig.validatiedatum' (vehicle validation date) is in the future.                                                  |                                   | A                   |
| 400    | G110 | 'andereWerkzaamheden.begintijdstip' (otherWorks.start time) is missing.                                                         |                                   | A                   |
| 400    | G111 | Value of 'andereWerkzaamheden.begintijdstip' (otherWorks.start time) does not conform to the format.                            |                                   | A                   |
| 400    | G120 | 'andereWerkzaamheden.eindetijdstip' (otherWorks.endtime) is missing.                                                            |                                   | A                   |
| 400    | G121 | Value of 'andereWerkzaamheden.eindetijdstip' (otherWorks.endtime) does not conform to the format.                               |                                   | A                   |
| 400    | G122 | 'andereWerkzaamheden.eindetijdstip' (otherWorks.endtime) is before 'andereWerkzaamheden.starttijdstip'  (otherWorks.start time) |                                   | A                   |
| 400    | G123 | Value of 'andereWerkzaamheden.eindetijdstip' (otherWorks.endtime) is after 'dienst.aanmeldtijdstip' (service.registration time) |                                   | A                   |
| 400    | G130 | 'locatie' (location) is missing.                                                                                                |                                   | C,I                 |
| 400    | G131 | 'locatie.breedtegraad' (location.latitude) is missing.                                                                          |                                   | C,I                 |
| 400    | G132 | Value of 'locatie.breedtegraad' (location.latitude) does not conform to the format.                                             |                                   | C,I                 |
| 400    | G133 | 'locatie.lengtegraad' (location.longitude) is missing.                                                                          |                                   | C,I                 |
| 400    | G134 | Value of 'locatie.lengtegraad' (location.longitude) does not conform to the format.                                             |                                   | C,I                 |
| 400    | G140 | 'afstand' (distance) is missing.                                                                                                |                                   | D                   |
| 400    | G141 | Value of 'afstand' (distance) does not conform to the format.                                                                   |                                   | D                   |
| 400    | G150 | 'ritprijs' (fare) is missing.                                                                                                   |                                   | D                   |
| 400    | G151 | Value of 'ritprijs' (fare) does not conform to the format.                                                                      |                                   | D                   |
| 400    | G160 | Value of path parameter 'rit' (ride) does not conform to the format.                                                            |                                   | D                   |
| 400    | G170 | Value of path parameter 'pauze' (break) does not conform to the format.                                                         |                                   | F                   |
| 400    | G180 | 'gebeurtenistijdstip' (event time) is missing.                                                                                  |                                   | I                   |
| 400    | G181 | Value of 'gebeurtenistijdstip' (event time) does not conform to the format.                                                     |                                   | I                   |
| 400    | G182 | Value of 'gebeurtenistijdstip' (event time) is in the future.                                                                   |                                   | I                   |
| 400    | G190 | 'gebeurteniscode' (event code) is missing.                                                                                      |                                   | I                   |
| 400    | G191 | Value of 'gebeurteniscode' (event code) does not conform to the format.                                                         |                                   | I                   |


### 3.16.3 Foutmeldingen wegens verwerken inhoud (Error messages due to processing content)


| status | Code     | Text                                                                                                              | Explanation                                                                                                                                                       | API Call Codes |
| ------ | -------- | ----------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------- |
| 400    | DF01     | Value of 'aanmeldtijdstip' (registration time) is within another service.                                         | A service can only be booked if the start time of the service does not overlap with a cancelled service for the same driver.                                      | A              |
| 400    | DF02     | Value of 'id' (id) is not unique.                                                                                 | Identification must be unique                                                                                                                                     | A,C,E,I        |
| 400    | DF03     | Service cannot be found based on the specified ID.                                                                | A report within a service can only be made if the service can be found.                                                                                           | B,C,D,E,F,I    |
| 400    | DF04     | Service has already been logged out.                                                                              | A service can only be unsubscribed if the service has not already been unsubscribed.                                                                              | B              |
| 400    | DF05 (1) | There are unreported transactions on this service.                                                                | A service can be logged out when all transactions on that service have been logged out.                                                                           | B              |
| 400    | DF09     | Value of 'afmeldtijdstip' (log-out time) is before 'aanmeldtijdstip' (registration time) of the service           | A service cannot be unsubscribed for a time before the login time                                                                                                 | B              |
| 400    | DF10     | Value of 'afmeldtijdstip' (log-out time) is before 'afmeldtijdstip' (log-out time) of transactions in the service | A logout time for a service cannot precede any logout or logout times for transactions within the service.                                                        | B              |
| 400    | DF11     | Value of 'afmeldtijdstip' (log-out time) falls within another service                                             | This can only occur with services provided subsequently.                                                                                                          | B              |
| 400    | VF01     | Value of 'aanmeldtijdstip' (registration time) is for 'dienst.aanmeldtijdstip' (service.login time) .             | On-duty work cannot commence before the on-duty start date                                                                                                        | C,E            |
| 400    | VF02     | Transaction cannot be found based on the specified ID.                                                            | A transaction (trip/break) can only be cancelled if it has been registered.                                                                                       | D,F            |
| 400    | VF03     | Transaction has already been cancelled.                                                                           | A cancelled transaction (trip/break) cannot be cancelled.                                                                                                         | D,F            |
| 400    | VF04     | Value of 'afmeldtijdstip' (log-out time) is for 'aanmeldtijdstip' (registration time) of transaction.             | A transaction within a service cannot end before the transaction begins                                                                                           | D,F            |
| 400    | VF05     | The maximum number of transactions has been reached for this service.                                             | The number of transactions per service is limited to 100.                                                                                                         | C,E            |
| 400    | VF06     | Value of 'aanmeldtijdstip' (registration time) of break is within trip or break.                                  | A break may not overlap with another action.                                                                                                                      | E              |
| 400    | VF07     | Value of 'aanmeldtijdstip' (registration time) of trip is within reported break.                                  | A ride cannot be started if: • a break has previously been registered and not cancelled; • the registration time falls within a registered and cancelled break. | C              |
| 400    | VF08     | Value of 'afmeldtijdstip' (log-out time) of break is within trip or break.                                        | A break may not overlap with another action. Note: This error can only occur if a break is reported afterward.                                                    | F              |
| 400    | VF09     | Value of 'afmeldtijdstip' (log-out time) of ride not allowed, break during ride.                                  | A ride cannot be cancelled if this would cause a break during the ride.                                                                                           | D              |
| 400    | VF10     | Transaction not found within the specified service                                                                | A trip or break is cancelled whose ID is not known to the service                                                                                                 | D,F            |
| 400    | VF11     | Value of 'aanmeldtijdstip' (registration time) of break is for registered transaction                             | It is not permitted to report a break with a check-in time before the check-in time of another operation.                                                         | E              |
| 400    | BF01     | The maximum number of events has been reached for this service.                                                   | The number of events per service is capped at 100.                                                                                                                | C,E            |
| 400    | OF01     | Maximum number of requests reached                                                                                | There is a maximum number of 500 requests per day per ICT service provider.                                                                                       | J              |
| 404    | OF02     | No driver number found for the specified driver's license                                                         | No driver number was found for submitting the driver's license.                                                                                                   | J              |

(1): With this code the relevant transaction(s) are also returned in the response.


- - -

# 4 TECHNICAL REQUIREMENTS

## 4.1 Conventions

### 4.1.1 JSON conventions

The Central application must submit messages to the endpoints of the CDT Notifications API using **JSON** (JavaScript Object Notation) messages and **REST** (Representational State Transfer). Fields that have no value are **omitted**.

### 4.1.2 Encoding

The character-encoding standard for the messages is **UTF-8**.

### 4.1.3 Case sensitivity

The CDT backend service is **CASE SENSITIVE**.

### 4.1.4 Date/time

The **IETF RFC 3339** standard is used for date and time, specifically the specification of the 'date-time' value. All times are in **UTC**.

### 4.1.5 Message traffic

Message traffic is handled **synchronously**. If there is a **time-out** (`> 15 seconds`), the Central application must resubmit the message.

### 4.1.6 Endpoints

All endpoints are specified in the OpenAPI specification

REF−1

.

## 4.2 Data timeliness

Messages must be supplied to the CDT backend service **without delay**. Each message contains two time stamps: the moment the event occurred (`aanmeldtijdstip` or `afmeldtijdstip`) and the moment the message about this action is created (`registratietijdstip`). The ICT service provider is responsible for ensuring that messages are submitted in **chronological order of recording time**.

## 4.3 Availability and performance

### 4.3.1 Availability

The API has a guaranteed availability of **98%**.

### 4.3.2 Performance

The target time for handling a message is **< 2 seconds**.

- - -

# 5 LOGGING AND CONNECTION MONITORING

## 5.1 Logging

The ICT service provider is expected to **log** all transactions on the CDT Notifications API, including the response from ILT. This data must be kept for a **minimum of 1 month**.

## 5.2 Connection monitoring

To monitor whether a connection is possible, the ICT service provider sends a `GET` to **`https://[host]/v2/verbinding`** if no other notification has been sent for longer than **60 seconds**, to which ILT will return a `200 OK`. **NB:** if the call to this endpoint fails, other transactions may not be called until this call succeeds again.

- - -

# 6 ERROR HANDLING

## 6.1 General

If a message related to a service is rejected, chronologically following messages for the same service must be **held** until the rejected message is successfully processed.

If the `/verbinding` endpoint returns a **`500-error`**, a new call to `/verbinding` must be made every minute until this call succeeds.

## 6.2 Error in calling service-related endpoints

| Status code | Recommended actions. |
| --- | --- |
| **403** | Unauthorized call. Correction is necessary for a new attempt. |
| **400** | For a 400 due to errors in the header or the message: first correct the error and then send a new message with the corrected data. |
| **50x** | The CDT Notifications API is not reachable. Try again after 1 minute (same message body, different time stamp, same messageId). |



## 6.3 Error in calling /verbinding

| Status code | Recommended actions. |
| --- | --- |
| **40x** | An error was made in the call. Try again with a new message. |
| **50x** | The CDT Notifications API is not reachable. Try again after 1 minute with a new call to /verbinding. |



## 6.4 Duplicate detection

There is **no duplicate detection**, a message submitted for the second time will be functionally rejected.

- - -

# 7 AUTHENTICATION AND INFORMATION SECURITY

## 7.1 Authentication

### 7.1.1 PKI Certificates

Authentication takes place using **PKI government server certificates** and **client certificates** at the ICT service provider.

### 7.1.2 Driving license authentication

With every service that is started, the driver's **Dutch driving license** must be **electronically** authenticated via **NFC** by performing the 'Active Authentication'

REF−2

.

### 7.1.3 Vehicle registration certificate authentication

When registering a vehicle for a carrier, the vehicle's **vehicle registration certificate** must be authenticated by means of the 'Active Authentication'

REF−3

.

## 7.2 Information security

Only **previously released IP addresses** can submit messages to the secure API gateway.

### 7.2.1 Transport Layer Security (TLS)

Traffic takes place over **TLS** with certificates on the sending and receiving sides. The most current guidelines of the NCSC are leading.

### 7.2.2 API-keys

The ILT issues an **API-key** (`ext_key` header) per ICT service provider; the ICT service provider uses the API-key to identify itself at the ILT API gateway.

## 7.3 Headers

The CDT Notifications API expects the following headers:

| Header | Value | example |
| --- | --- | --- |
| **Accept** | Fixed value | `Accept: application/json` |
| **Content-Type** | Fixed value | `Content-Type: application/json` |
| **Dienstverlener** | UUID | `Dienstverlener: <uuid>` |
| **ext\_key** | UUID | `ext_key: <uuid>` |
| **Bericht-Id** | UUID | `Bericht-ld: <uuid>` |
| **Verzendtijdstip** | `date-time` in UTC conforming to IETF RFC3339 | `Verzendtijdstip: 20241120T17:51:01.556Z` |
| **Softwareversie-Registratiemiddel** | Max 20 positions | `Softwareversie-Registratiemiddel: v1.0.3` |
| **Softwareversie-Centrale-Applicatie** | Max. 20 positions | `Softwareversie-Centrale-Applicatie: v12.6.5` |


