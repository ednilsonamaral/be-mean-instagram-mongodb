# MongoDb - Projeto Final  
**Autor:** Ednilson Amaral  
**Data** Date.now() //em timestamp


## Para qual sistema você usaria o MogoDB (diferente desse)?


## Qual a modelagem da sua coleção de `users`?  

```  
users{  
	name: String,  
	bio: String,  
	email: String,  

	auth: [{  
		username: String,  
		password: String,  
		last_access: Date,  
		online: boolean,  
		disable: boolean,  
		hash_token: String  
	}],  

	date_register: Date,  
	avatar_path: String  
}  
```


## Qual a modelagem da sua coleção de `projects`?  

```  
projects{  
	name: String,  
	description: String,  
	date_begin: Date,  
	date_dream: Date,  
	date_end: Date,  
	visible: boolean,  
	realocate: boolean,  
	expired: boolean,  
	visualizable_mod,  

	tags: [],  

	members: [{  
		name: String,  
		type_member: String,  
		notify: boolean  
	}],  

	goals: [{  
		name: String,  
		description: String,  
		date_begin: Date,  
		date_dream: Date,  
		date_end: Date,  
		realocate: boolean,  
		expired: booelan,  

		tags: [],  

		activities: [{  
			name: String,  
			description: String,  
			date_begin: Date,  
			date_dream: Date,  
			date_end: Date,  
			realocate: boolean,  
			expired: boolean  
		}],  
	}],  

	comments: [{  
		user_name: users.name,  
		text: String,  
		date_comment: String  
	}]  
}  
```


## Create - cadastro  

### Usuários  

Primeiramente, fiz um único cadastro para teste:  

```  
test> use be-mean-mongodb
switched to db be-mean-mongodb

be-mean-mongodb> var json = {  
	"name": "Ednilson Amaral",  
	"bio": "Desenvolvedor Frontend",  
	"email": "ednilsonamaral.ti@gmail.com",  
	"auth": [  
		{  
			"username": "ednilson",  
			"password": "123456",  
			"last_access": Date.now(),  
			"online": false,  
			"disable": false,  
			"hash_token": "oi",  
		}  
	],    
	"date_register": Date.now(),  
	"avatar_path": "oi"}  

be-mean-mongodb> json  
{  
  "name": "Ednilson Amaral",  
  "bio": "Desenvolvedor Frontend",  
  "email": "ednilsonamaral.ti@gmail.com",  
  "auth": [  
    {  
      "username": "ednilson",  
      "password": "123456",  
      "last_access": 1450115832467,  
      "online": false,  
      "disable": false,  
      "hash_token": "oi"  
    }  
  ],  
  "date_register": 1450115832467,  
  "avatar_path": "oi"  
}  

be-mean-mongodb> db.users.save(json)  
Inserted 1 record(s) in 145ms  
WriteResult({  
  "nInserted": 1  
})  

be-mean-mongodb> db.users.find()  
{  
  "_id": ObjectId("566f030c0b5e9fe3f7610e1d"),  
  "name": "Ednilson Amaral",  
  "bio": "Desenvolvedor Frontend",  
  "email": "ednilsonamaral.ti@gmail.com",  
  "auth": [  
    {  
      "username": "ednilson",  
      "password": "123456",  
      "last_access": 1450115832467,  
      "online": false,  
      "disable": false,  
      "hash_token": "oi"  
    }  
  ],  
  "date_register": 1450115832467,  
  "avatar_path": "oi"  
}  
Fetched 1 record(s) in 3ms  
```  

Após essa inserção de teste, inserir os 9 usuários restantes através de um `mongoimport`:  

```  
ednilson@EDNILSON-NB:/var/www/be-mean-instagram-mongodb$ mongoimport --db be-mean-mongodb --collection users --file users.json  
2015-12-14T16:17:03.615-0200	connected to: localhost  
2015-12-14T16:17:03.618-0200	imported 9 documents  
```  

Eu não utilizei o parâmetro `--drop` junto do `mongoimport` devido que o registro que já tinha na coleção `users` também será utilizado posteriormente.


## Retrieve - busca


## Update - alteração


## Delete - remoção


## Sharding
// coloque aqui todos os comandos que você executou


## Replica
// coloque aqui todos os comandos que você executou