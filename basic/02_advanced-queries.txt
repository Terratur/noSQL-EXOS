# Exercices Avancés MongoDB avec la Base de Données Titanic

## Exo 1 :

`test> use TitanicDB
switched to db TitanicDB
TitanicDB> db.createCollection("Passengers")
{ ok: 1 }
`
`test> use TitanicDB
switched to db TitanicDB
TitanicDB> show collections
Passengers
TitanicDB> db.Passengers.countDocuments()
418
`
## Exo 2 :

`db.Passengers.countDocuments()
`
`TitanicDB> db.Passengers.countDocuments({ Survived: 1 })
152
`
`TitanicDB> db.Passengers.countDocuments({ Sex: "female" })
152
`
`TitanicDB> db.Passengers.countDocuments({ Parch: { $gte: 3 } })
9
`

## Exo 3 :

`TitanicDB> db.Passengers.updateMany({ $or: [ { Embarked: null }, { Embarked: "" } ] }, { $set: { EmbarEmbarked: "S" } })
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 0,
  modifiedCount: 0,
  upsertedCount: 0
}`

`TitanicDB> db.Passengers.updateMany({ Survived: 1 },{ $set: { rescued: true } })
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 152,
  modifiedCount: 152,
  upsertedCount: 0
}
`
## Exo 4 

`TitanicDB> db.Passengers.find({ Age: { $ne: null } },{ Name: 1, Age: 1, _id: 0 }).sort({ Age: 1 }).limit(10)
[
  { Name: 'Dean, Miss. Elizabeth Gladys Millvina""', Age: 0.17 },
  { Name: 'Danbom, Master. Gilbert Sigvard Emanuel', Age: 0.33 },
  { Name: 'Peacock, Master. Alfred Edward', Age: 0.75 },
  { Name: 'Aks, Master. Philip Frank', Age: 0.83 },
  { Name: 'West, Miss. Barbara J', Age: 0.92 },
  { Name: 'Laroche, Miss. Louise', Age: 1 },
  { Name: 'Klasen, Miss. Gertrud Emilia', Age: 1 },
  { Name: 'Sandstrom, Miss. Beatrice Irene', Age: 1 },
  { Name: 'Wells, Master. Ralph Lester', Age: 2 },
  { Name: 'Rosblom, Miss. Salli Helena', Age: 2 }
]
`
`TitanicDB> db.Passengers.find({ Survived: 0, Pclass: 2 },{ Name: 1, _id: 0 })
[
  { Name: 'Myles, Mr. Thomas Francis' },
  { Name: 'Caldwell, Mr. Albert Francis' },
  { Name: 'Howard, Mr. Benjamin' },
  { Name: 'Keane, Mr. Daniel' },
  { Name: 'Louch, Mr. Charles Alexander' },
  { Name: 'Jefferys, Mr. Clifford Thomas' },
  { Name: 'Pulbaum, Mr. Franz' },
  { Name: 'Mangiavacchi, Mr. Serafino Emilio' },
  { Name: 'McCrae, Mr. Arthur Gordon' },
  { Name: 'Aldworth, Mr. Charles Augustus' },
  { Name: 'Lamb, Mr. John Joseph' },
  { Name: 'Wells, Master. Ralph Lester' },
  { Name: 'Weisz, Mr. Leopold' },
  { Name: 'Stanton, Mr. Samuel Ward' },
  { Name: 'Swane, Mr. George' },
  { Name: 'Bowenur, Mr. Solomon' },
  { Name: 'Schmidt, Mr. August' },
  { Name: 'Beauchamp, Mr. Henry James' },
  { Name: 'Lahtinen, Rev. William' },
  { Name: 'Peruschitz, Rev. Joseph Maria' }
]
Type "it" for more
`
## Exo 5 :

`TitanicDB> db.Passengers.deleteMany({Survived: 0,$or: [ { Age: null }, { Age: "" } ]})
{ acknowledged: true, deletedCount: 61 }
`
## Exo 6 : 

echec pour le moment 

## Exo 7

`TitanicDB> db.Passengers.deleteMany({$or: [ { Ticket: null }, { Ticket: "" } ]})
{ acknowledged: true, deletedCount: 0 }
`
