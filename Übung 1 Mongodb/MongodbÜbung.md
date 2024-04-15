# Aufgabe 1
Speichern Sie in MongoDB folgende Informationen:
• Die Studiengänge AIN, WIN und MSI mit folgenden Attributen
o Kürzel, z.B. AIN, WIN
o Name, z.B. Angewandte Informatik, Wirtschaftsinformatik
o Abschluss z.B. Bachelor, Master
• Mindestens 10 Vorlesungen pro Studiengang mit den Attributen
o Name
o Dozent
o Semester
o Studiengang
o SWS
o ECTS
Definieren Sie zunächst ein JSON-Schema für die Collections. Speichern Sie
anschliessend die Daten und speichern Sie die Beziehungen zwischen
Studiengängen und Vorlesungen durch Referenzen (DBRef) ab.

Schema:

{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://example.com/product.schema.json",
  "title": "Aufgabe 1",
  "description": "Aufgabe 1 MongoDB Übung KAD",
  "type": "object",
    "properties": {
        "abbreviation": {
            "description": "Abbreviation of the study subject",
            "type": "string"
        },
        "name": {
            "description": "Name of the of the study subject",
            "type": "string"
        },
        "level": {
            "description": "Degree level",
            "type": "string"
        },
        "courses": {
            "description": "Associated courses to the studies",
            "type": "object",
            "properties": {
                "name": {
                    "description": "Name of the course",
                    "type": "string"
                },
                "lecturer"{
                    "description": "Lecturer of the course",
                    "type": "string"
                },
                "semester": {
                    "description": "Semester that the course takes place during studies",
                    "type": "integer"
                },
                "studies": {
                    "description": "Studies the course is associated with",
                    "type": "string"
                },
                "sws": {
                    "description": "Weekly lecture hours  of the course",
                    "type": "integer"
                },
                "ects": {
                    "description": "ects credits students get from taking the course",
                    "type": "integer"
                }

            },
            "required": ["name", "lecturer", "semester", "studies", "sws", "ects"] 
        }
    },
    "required" : ["abbreviation", "name", "level"]
}


{
    "abbreviation": "AIN",
    "name": "Angewandte Informatik",
    "level": "Bachelor",
    "courses": {"name": "test", "lecturer" : "asd", "semester" : 2, "studies" : "asd", "sws": 2, "ects": 5}
}
{
    "abbreviation": "WIN",
    "name": "Wirtschaftsinformatik",
    "level": "Bachelor",
    "courses": {}
},
{
    "abbreviation": "MSI",
    "name": "Informatik",
    "level": "Master",
    "courses": {}
},



# Aufgabe 2
Geben Sie Anfragen für folgende Fragen an
a) Welche Studiengänge haben einen Bachelor als Abschluss? Es sollen nur die
Studiengangskürzel ausgegeben werden.
b) Ermitteln Sie, welche AIN-Vorlesungen weniger als 5 SWS besitzen. Es sollen nur
die Vorlesungsnamen alphabetisch sortiert ausgegeben werden.
c) Bei welchen Vorlesungen ist SWS größer als ECTS?
d) Wie viele AIN-Vorlesungen halten die einzelnen Professoren? Es soll nur der
Professorname und die SWS-Summe ausgegeben werden.
e) Welcher Professor hält am meisten SWS in AIN-Vorlesungen?


# Aufgabe 3
Gegeben ist die unten angegebene Datenbank.
Geben Sie für die folgenden Suchanfragen Queries in MongoDB an.
a)
Welche Abteilungen haben keine Mitarbeiter?
b)
Wer hat einen Chef der jünger ist als er selbst? Vergleichen Sie die Anfrage mit
einem äquivalenten SQL-Befehl.
c)
Welche Abteilung hat durchschnittlich die jüngsten Mitarbeiter? Es sollen nur die
Abteilungsnummer und der Abteilungsname ausgegeben werden.