# BDD mongodb PokémonGo

petit setup de mongodb community en suivant la doc : https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-debian/

`
root@docker:~# mongoimport --type csv --file pokemonGO.csv --headerline --db poké --collection user 
2025-06-29T14:07:11.993+0200    Failed: field 'Pokemon No.' cannot end with a '.'
2025-06-29T14:07:11.993+0200    0 document(s) imported successfully. 0 document(s) failed to import
`
ça commence bien, mais gpt m'as fait un petit sed aux petis ognions

`sed '1s/Pokemon No\./PokemonNo/' pokemonGO.csv > cleaned.csv
`
une fois le csv corrigée je peux donc l'importer :

`root@docker:~# mongoimport --type csv --file cleaned.csv --headerline --db poke --collection users 
2025-06-29T14:09:46.488+0200    connected to: mongodb://localhost/
2025-06-29T14:09:46.505+0200    151 document(s) imported successfully. 0 document(s) failed to import.`

J'ai jamais fait de mongodb donc je découvre, je risque faire quelques erreurs et de pas tout capter d'un coup :)

## Exo 1:

petit récap de ce que j'ai fait :

`
root@docker:~# mongosh
Current Mongosh Log ID: 6861324be4ef102af3baa8b8
Connecting to:          mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+2.5.3
Using MongoDB:          8.0.11
Using Mongosh:          2.5.3
For mongosh info see: https://www.mongodb.com/docs/mongodb-shell/
`

`
test> show dbs
admin    40.00 KiB
config  108.00 KiB
local    40.00 KiB
poke     48.00 KiB
test> use poke
switched to db poke
`
`poke> db.createCollection("Pokemons")
{ ok: 1 }
`

`poke> show collections
Pokemons
users
`

## Exo 2 :

Je corrige ma connerie :

`root@docker:~# mongoimport --type csv --file cleaned.csv --headerline --db poke --collection Pokemons 2025-06-29T14:43:33.453+0200    connected to: mongodb://localhost/
2025-06-29T14:43:33.456+0200    151 document(s) imported successfully. 0 document(s) failed to import.`
Je test pour voir si la collection Pokemons est bien remplie

`poke> db.Pokemons.findOne()
{
  _id: ObjectId('686134f5f61e264630004279'),
  PokemonNo: 1,
  Name: 'Bulbasaur',
  'Type 1': 'Grass',
  'Type 2': 'Poison',
  'Max CP': 1079,
  'Max HP': 83,
  'Image URL': 'http://cdn.bulbagarden.net/upload/thumb/2/21/001Bulbasaur.png/250px-001Bulbasaur.png'
}
`
C'est valide 

## Exo 3 

`poke> db.Pokemons.find({ "Type 1" : "Fire" })
[
  {
    _id: ObjectId('686134f5f61e26463000427c'),
    PokemonNo: 4,
    Name: 'Charmander',
    'Type 1': 'Fire',
    'Type 2': '',
    'Max CP': 962,
    'Max HP': 73,
    'Image URL': 'http://cdn.bulbagarden.net/upload/thumb/7/73/004Charmander.png/250px-004Charmander.png'
  },
...
`
`poke> db.Pokemons.find({ Name: "Pikachu" })
[
  {
    _id: ObjectId('686134f5f61e264630004290'),
    PokemonNo: 25,
    Name: 'Pikachu',
    'Type 1': 'Electric',
    'Type 2': '',
    'Max CP': 894,
    'Max HP': 67,
    'Image URL': 'http://cdn.bulbagarden.net/upload/thumb/0/0d/025Pikachu.png/250px-025Pikachu.png'
  }
]
`

## Exo 4 :

`poke> db.Pokemons.updateOne({ Name: "Pikachu"}, { $set: {"Max CP": 900} }) 
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}
poke> db.Pokemons.find({ Name: "Pikachu" })
[
  {
    _id: ObjectId('686134f5f61e264630004290'),
    PokemonNo: 25,
    Name: 'Pikachu',
    'Type 1': 'Electric',
    'Type 2': '',
    'Max CP': 900,
    'Max HP': 67,
    'Image URL': 'http://cdn.bulbagarden.net/upload/thumb/0/0d/025Pikachu.png/250px-025Pikachu.png'
  }
]
`
## Exo 5 :

`poke> db.Pokemons.deleteOne({ Name: "Bulbasaur" })
{ acknowledged: true, deletedCount: 1 }
poke> db.Pokemons.find({ Name: "Bulbasaur" })
...
poke>`


