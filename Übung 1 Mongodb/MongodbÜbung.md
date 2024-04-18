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

db.studyprogramm.find({level: "Bachelor"}, {_id:0, abbreviation:1})

b) Ermitteln Sie, **welche AIN-Vorlesungen weniger als 5 SWS besitzen.** Es sollen nur
die Vorlesungsnamen alphabetisch sortiert ausgegeben werden.

db.courses.find({$where: "this.sws > this.ects && this.studyprogramm == 'AIN'"}, {"_id":0, "name":1}).sort({"name":1})

c) Bei welchen Vorlesungen ist SWS größer als ECTS?

db.courses.find({$where: "this.sws > this.ects"})

d) Wie viele AIN-Vorlesungen halten die einzelnen Professoren? Es soll nur der
Professorname und die SWS-Summe ausgegeben werden.

db.courses.aggregate([
    {$match: { 
        "studyprogram.$ref": "studyprogramm",
        "studyprogram.$id": db.studyprogramm.findOne({"abbreviation": "AIN"})._id
    }},
    {"$project" : {"lecturer" : 1 , "sws" : 1 }},
    {$group: { _id: "$lecturer", "count": { $sum: 1 }, sws: {$sum: "$sws"}}},
    {"$sort" : {"count" : -1 }},
])

e) Welcher Professor hält am meisten SWS in AIN-Vorlesungen? wie maX?

db.courses.aggregate([
    {$match: { 
        "studyprogram.$ref": "studyprogramm",
        "studyprogram.$id": db.studyprogramm.findOne({"abbreviation": "AIN"})._id
    }},
    {"$project" : {"lecturer" : 1 , "sws" : 1 }},
    {$group: { _id: "$lecturer", "count": { $sum: 1 }, sws: {$sum: "$sws"}}},
    {"$sort" : {"sws" : -1 }},
    {"$limit" : 1}
])

igw nur größtes drin lassen?


# Aufgabe 3
Gegeben ist die unten angegebene Datenbank.
Geben Sie für die folgenden Suchanfragen Queries in MongoDB an.
a)
Welche Abteilungen haben keine Mitarbeiter?

db.abt.aggregate([
    {
    $project: { "name": 1, "anr": 1, "ort": 1 } 
    },
    {
        $lookup: {
            from: "pers",
            localField: "_id",
            foreignField: "abteilung.$id",
            as: "mitarbeiter"
        }
    },
    { 
        $match: { mitarbeiter: { $size: 0 } } 
    }
]);

b)
Wer hat einen Chef der jünger ist als er selbst? Vergleichen Sie die Anfrage mit
einem äquivalenten SQL-Befehl.

db.pers.aggregate([
    {    
        $lookup: {
            from: "pers",
            localField: "vorgesetzter.$id",
            foreignField: "_id",
            as: "vorgesetzter"
        }    
    },
  { $unwind: "$vorgesetzter" },
   {$match: { "jahrg" : { $ne : null }}},
  { $match: { $expr: { $gt: ["$vorgesetzter.jahrg", "$jahrg"] } } }
    
    
])

SELECT * 
FROM pers as pers1
LEFT OUTER JOIN pers as pers2 ON pers1._id = pers2._id
WHERE pers1.jahrg IS NOT NULL AND pers.jahrg < pers2.jahrg




c)
Welche Abteilung hat durchschnittlich die jüngsten Mitarbeiter? Es sollen nur die
Abteilungsnummer und der Abteilungsname ausgegeben werden.

db.abt.aggregate([
    {
        $lookup:{
            from: "pers",
            localField: "_id",
            foreignField: "abteilung.$id",
            as: "mitarbeiter"
       }
    },
  { $unwind: "$mitarbeiter"},
  { $group: {_id: {name: "$name", anr: "$anr", }, employeesAvgJahrg: { $avg: "$mitarbeiter.jahrg"}}},
  { $sort: { employeesAvgJahrg: -1 } }, 
  { $limit: 1 },
  { $project: { "name": "$_id.name", "anr": "$_id.anr", "_id": 0 } }
  
])