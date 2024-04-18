db.createCollection("studyprogramm", {
        validator: {
            $jsonSchema: {
                bsonType: "object",
                title: "Studyprogramm Object Validation",
                required: ["abbreviation", "name", "level"],
                properties: {
                    abbreviation: {
                        bsonType: "string",
                        description: "must be a string and is required"
                    },
                    name: {
                        bsonType: "string",
                        description: "must be a string and is required"
                    },
                    level: {
                        bsonType: "string",
                        description: "must be a string and is required"
                        
                    }
                    
                }
                
            }
        }
    }
)

db.createCollection("courses", {
    validator: {
        $jsonSchema: {
            bsonType: "object",
            required: ["name", "lecturer", "semester", "studyprogram", "sws", "ects"],
            properties: {
                name: {
                    bsonType: "string",
                    description: "must be a string and is required"
                },
                lecturer: {
                    bsonType: "string",
                    description: "must be a string and is required"
                },
                semester: {
                    bsonType: "int",
                    description: "must be a int and is required"
                },
                studyprogram: {
                    bsonType: "object",
                    description: "must be a objectId and is required",
                    properties: {
                        "$ref": {
                            bsonType: "string"
                        },
                        "$id": {
                            bsonType: "objectId"
                        }
                    }
                },
                sws: {
                    bsonType: "int",
                    description: "must be a int and is required"
                },
                ects: {
                    bsonType: "int",
                    description: "must be a int and is required"
                }
            }
        }
    }
} ) 


s1={
    "abbreviation": "AIN",
    "name": "Angewandte Informatik",
    "level": "Bachelor"
}
s2={
    "abbreviation": "WIN",
    "name": "Wirtschaftsinformatik",
    "level": "Bachelor"
}
s3={
    "abbreviation": "MSI",
    "name": "Informatik",
    "level": "Master"
}

db.studyprogramm.insertOne(s1)
db.studyprogramm.insertOne(s2)
db.studyprogramm.insertOne(s3)

ain1={
        "name": "Software Engineering", 
        "lecturer" : "Ralf Schimkat", 
        "semester" : 1,
        "sws": 4, 
        "ects": 6,
        "studyprogram": neiw DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "AIN"})._id)
    }
ain2={
        "name": "Datenstrukturen", 
        "lecturer" : "Jane Smith", 
        "semester" : 1, 
        "sws": 3, 
        "ects": 5,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "AIN"})._id)
    }
ain3={
        "name": "Netzwerktechnik", 
        "lecturer" : "Robert Johnson", 
        "semester" : 2, 
        "sws": 2, 
        "ects": 4,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "AIN"})._id)
    }
ain4={
        "name": "Betriebsysteme", 
        "lecturer" : "Emily Davis", 
        "semester" : 2,
        "sws": 3, 
        "ects": 5,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "AIN"})._id)
    }
ain5={
        "name": "Databanken 1", 
        "lecturer" : "Oliver Eck", 
        "semester" : 2,
        "sws": 2, 
        "ects": 5,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "AIN"})._id)
    }
ain6={
        "name": "Databanken 2", 
        "lecturer" : "Oliver Eck", 
        "semester" : 3,
        "sws": 2, 
        "ects": 5,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "AIN"})._id)
    }
ain7={
        "name": "Web Entwicklung", 
        "lecturer" : "Sarah Brown", 
        "semester" : 3, 
        "sws": 3, 
        "ects": 5,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "AIN"})._id)
    }
ain8={
        "name": "Mathematik 1", 
        "lecturer" : "Barbara Staehle", 
        "semester" : 1, 
        "sws": 3, 
        "ects": 5,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "AIN"})._id)
    }    ,
ain9={
        "name": "Mathematik 2", 
        "lecturer" : "Barbara Staehle", 
        "semester" : 3, 
        "sws": 3, 
        "ects": 5,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "AIN"})._id)
    }
ain10={
        "name": "Projektmanagement", 
        "lecturer" : "Emily Davis", 
        "semester" : 3,
        "sws": 4, 
        "ects": 2,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "AIN"})._id)
    }
db.courses.insertOne(ain1)
db.courses.insertOne(ain2)
db.courses.insertOne(ain3)
db.courses.insertOne(ain4)
db.courses.insertOne(ain5)
db.courses.insertOne(ain6)
db.courses.insertOne(ain7)
db.courses.insertOne(ain8)
db.courses.insertOne(ain9)
db.courses.insertOne(ain10)

win1={
        "name": "Business Administration", 
        "lecturer" : "John Doe", 
        "semester" : 1,
        "sws": 4, 
        "ects": 2,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "WIN"})._id)
    }
win2={
        "name": "Accounting", 
        "lecturer" : "John Doe", 
        "semester" : 1,
        "sws": 3, 
        "ects": 5,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "WIN"})._id)
    }
win3={
        "name": "Economics", 
        "lecturer" : "Jane Smith", 
        "semester" : 2,
        "sws": 3, 
        "ects": 4,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "WIN"})._id)
    }
win4={
        "name": "Marketing", 
        "lecturer" : "Jane Smith", 
        "semester" : 2,
        "sws": 3, 
        "ects": 2,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "WIN"})._id)
    }
win5={
        "name": "Management Information Systems", 
        "lecturer" : "Robert Johnson", 
        "semester" : 3,
        "sws": 2, 
        "ects": 4,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "WIN"})._id)
    }
win6={
        "name": "Business Law", 
        "lecturer" : "Robert Johnson", 
        "semester" : 3,
        "sws": 3, 
        "ects": 5,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "WIN"})._id)
    }
win7={
        "name": "Finance", 
        "lecturer" : "Emily Davis", 
        "semester" : 4,
        "sws": 2, 
        "ects": 4,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "WIN"})._id)
    }
win8={
        "name": "Human Resources Management", 
        "lecturer" : "Emily Davis", 
        "semester" : 4,
        "sws": 3, 
        "ects": 5,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "WIN"})._id)
    }
win9={
        "name": "Operations Management", 
        "lecturer" : "Michael Miller", 
        "semester" : 5,
        "sws": 2, 
        "ects": 4,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "WIN"})._id)
    }
win10={
        "name": "Strategic Management", 
        "lecturer" : "Michael Miller", 
        "semester" : 5,
        "sws": 3, 
        "ects": 5,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "WIN"})._id)
    }
db.courses.insertOne(win1)
db.courses.insertOne(win2)
db.courses.insertOne(win3)
db.courses.insertOne(win4)
db.courses.insertOne(win5)
db.courses.insertOne(win6)
db.courses.insertOne(win7)
db.courses.insertOne(win8)
db.courses.insertOne(win9)
db.courses.insertOne(win10)

msi1={
        "name": "Agile Vorgehensmodelle", 
        "lecturer" : "Ralf Schimkat", 
        "semester" : 1,
        "sws": 3, 
        "ects": 5,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "MSI"})._id)
    }
msi2={
        "name": "Algorytmentechnik", 
        "lecturer" : "Georg Umlauf", 
        "semester" : 2,
        "sws": 4, 
        "ects": 3,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "MSI"})._id)
    }
msi3={
        "name": "Concurrency", 
        "lecturer" : "Oliver Haase", 
        "semester" : 2,
        "sws": 2, 
        "ects": 4,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "MSI"})._id)
    }
msi4={
        "name": "Seminar", 
        "lecturer" : "Georg Umlauf", 
        "semester" : 1,
        "sws": 3, 
        "ects": 5,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "MSI"})._id)
    }
msi5={
        "name": "Konzepte aktueller Datenbanksysteme", 
        "lecturer" : "Oliver Eck", 
        "semester" : 3, 
        "sws": 2, 
        "ects": 4,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "MSI"})._id)
    }
msi6={
        "name": "Mobile Kommunikation",  
        "lecturer" : "Rainer Müller", 
        "semester" : 1,
        "sws": 3, 
        "ects": 5,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "MSI"})._id)
    }
msi7={
        "name": "Machine Learning", 
        "lecturer" : "Jennifer Taylor", 
        "semester" : 1,
        "sws": 3, 
        "ects": 5,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "MSI"})._id)
    }
msi8={
        "name": "Cloud Application Development", 
        "lecturer" : "Joachim Eigelsperger", 
        "semester" : 1,
        "sws": 2, 
        "ects": 4,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "MSI"})._id)
    }
msi9={
        "name": "Cyber Security", 
        "lecturer" : "Jessica Martin", 
        "semester" : 1,
        "sws": 6, 
        "ects": 5,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "MSI"})._id)
    }
msi10={
        "name": "Stochastik", 
        "lecturer" : "Barbara Staehle", 
        "semester" : 1,
        "sws": 4, 
        "ects": 3,
        "studyprogram": new DBRef("studyprogramm", db.studyprogramm.findOne({"abbreviation": "MSI"})._id)
    }
db.courses.insertOne(msi1)
db.courses.insertOne(msi2)
db.courses.insertOne(msi3)
db.courses.insertOne(msi4)
db.courses.insertOne(msi5)
db.courses.insertOne(msi6)
db.courses.insertOne(msi7)
db.courses.insertOne(msi8)
db.courses.insertOne(msi9)
db.courses.insertOne(msi10)