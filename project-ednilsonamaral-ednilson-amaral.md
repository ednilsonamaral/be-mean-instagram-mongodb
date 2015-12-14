# MongoDb - Projeto Final  
**Autor:** Ednilson Amaral  
**Data:** Date.now() //em timestamp


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


### Projetos  

```  
ednilson@EDNILSON-NB:/var/www/be-mean-instagram-mongodb$ mongoimport --db be-mean-mongodb --collection projects --drop --file projects.json  
2015-12-14T17:13:16.227-0200	connected to: localhost  
2015-12-14T17:13:16.227-0200	dropping: be-mean-mongodb.projects  
2015-12-14T17:13:16.383-0200	imported 5 documents  
```  

O arquivo `projects.json`, contém:  

```  
{"name": "Projeto 1","description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit","date_begin": null,"date_dream": null,"date_end": null,"visible": false,"realocate": false,"expired": false,"visualizable_mod": null,"tags": ["web", "nfl", "be mean"],"members": [{"name": "Edson Arantes do Nascimento","type_member": "Tipo B","notify": false},{"name": "Carlitos Tevez","type_member": "Tipo C","notify": false},{"name": "Stephen Curry","type_member": "Tipo A","notify": false},{"name": "Aaron Rodgers","type_member": "Tipo A","notify": false},{"name": "Tom Brady","type_member": "Tipo A","notify": false}],"goals": [{"name": "Vagner Love","description": "Recebeu passe de Renato Augusto, goleiro adversário se adiantou e fez um golaço de cobertura!","date_begin": null,"date_dream": null,"date_end": null,"realocate": false,"expired": false,"tags": ["cobertura", "chapeuzinho", "por cima"],"activities": [{"name": "Atividade 1","description": "Descrição completa da atividade 1","date_begin": null,"date_dream": null,"date_end": null,"realocate": false,"expired": false},{"name": "Atividade 2","description": "Descrição completa da atividade 2","date_begin": null,"date_dream": null,"date_end": null,"realocate": false,"expired": false}]}],"comments": [{"user_name": "Ednilson Amaral","text": "Belo gol de Vagner Love. Demorou para desencantar, mas depois que desencantou, não pára mais de fazer gol! Isso é ótimo para ele e perfeito para o Corinthians, parça!","date_comment": null}]}  

{"name": "Projeto 2","description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit","date_begin": null,"date_dream": null,"date_end": null,"visible": false,"realocate": false,"expired": false,"visualizable_mod": null,"tags": ["zaga", "escanteio", "fundo"],"members": [{"name": "GPP","type_member": "Tipo B","notify": false},{"name": "Odel Bechkam Jr","type_member": "Tipo C","notify": false},{"name": "Victor Cruz","type_member": "Tipo A","notify": false},{"name": "Bill Bellicheck","type_member": "Tipo A","notify": false},{"name": "Luis Enrique","type_member": "Tipo A","notify": false}],"goals": [{"name": "Vermaleen","description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit","date_begin": null,"date_dream": null,"date_end": null,"realocate": false,"expired": false,"tags": ["jovem", "cabeça", "zagueiro"],"activities": [{"name": "Atividade 1","description": "Descrição completa da atividade 1","date_begin": null,"date_dream": null,"date_end": null,"realocate": false,"expired": false},{"name": "Atividade 2","description": "Descrição completa da atividade 2","date_begin": null,"date_dream": null,"date_end": null,"realocate": false,"expired": false}]}],"comments": [{"user_name": "Érica dos Santos","text": "Lorem ipsum dolor sit amet, consectetur adipisicing elit","date_comment": null}]}  

{"name": "Projeto 3","description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit","date_begin": null,"date_dream": null,"date_end": null,"visible": false,"realocate": false,"expired": false,"visualizable_mod": null,"tags": ["zaga", "ataque", "sozinho"],"members": [{"name": "PVC","type_member": "Tipo B","notify": false},{"name": "Everaldo Marques","type_member": "Tipo C","notify": false},{"name": "Carlão Barreto","type_member": "Tipo A","notify": false},{"name": "Mauro Betting","type_member": "Tipo A","notify": false},{"name": "Nivaldo Pietro","type_member": "Tipo A","notify": false}],"goals": [{"name": "Cristiano Ronaldo","description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit","date_begin": null,"date_dream": null,"date_end": null,"realocate": false,"expired": false,"tags": ["fila", "time", "cade"],"activities": [{"name": "Atividade 1","description": "Descrição completa da atividade 1","date_begin": null,"date_dream": null,"date_end": null,"realocate": false,"expired": false},{"name": "Atividade 2","description": "Descrição completa da atividade 2","date_begin": null,"date_dream": null,"date_end": null,"realocate": false,"expired": false}]}],"comments": [{"user_name": "Laís Ramos","text": "Lorem ipsum dolor sit amet, consectetur adipisicing elit","date_comment": null}]}  

{"name": "Projeto 4","description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit","date_begin": null,"date_dream": null,"date_end": null,"visible": false,"realocate": false,"expired": false,"visualizable_mod": null,"tags": ["vaca", "meia lua", "gremio"],"members": [{"name": "Juninho Pernambucano","type_member": "Tipo B","notify": false},{"name": "William","type_member": "Tipo C","notify": false},{"name": "Junior","type_member": "Tipo A","notify": false},{"name": "Luis Roberto","type_member": "Tipo A","notify": false},{"name": "Cleber Machado","type_member": "Tipo A","notify": false}],"goals": [{"name": "Ronaldinho Gaúcho","description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit","date_begin": null,"date_dream": null,"date_end": null,"realocate": false,"expired": false,"tags": ["meia lua", "drible da vaca", "tchau"],"activities": [{"name": "Atividade 1","description": "Descrição completa da atividade 1","date_begin": null,"date_dream": null,"date_end": null,"realocate": false,"expired": false},{"name": "Atividade 2","description": "Descrição completa da atividade 2","date_begin": null,"date_dream": null,"date_end": null,"realocate": false,"expired": false}]}],	"comments": [{"user_name": "Carlos Almeida","text": "Lorem ipsum dolor sit amet, consectetur adipisicing elit","date_comment": null}]}  

{  
	"name": "Projeto 5",  
	"description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit",  
	"date_begin": null,  
	"date_dream": null,  
	"date_end": null,  
	"visible": false,  
	"realocate": false,  
	"expired": false,  
	"visualizable_mod": null,  
  
	"tags": ["internacional", "barcelona", "espanhol"],  

	"members": [  
		{  
		"name": "Ronaldo",  
		"type_member": "Tipo B",  
		"notify": false  
		},  

		{  
		"name": "Milton Leite",  
		"type_member": "Tipo C",  
		"notify": false  
		},  

		{  
		"name": "Galvão Bueno",  
		"type_member": "Tipo A",  
		"notify": false  
		},  

		{  
		"name": "Eli Manning",  
		"type_member": "Tipo A",  
		"notify": false  
		},  
  
		{  
		"name": "Peyton Manning",  
		"type_member": "Tipo A",  
		"notify": false  
		}	  
	],  

	"goals": [{  
		"name": "Lionel Messi",  
		"description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit",  
		"date_begin": null,  
		"date_dream": null,  
		"date_end": null,  
		"realocate": false,  
		"expired": false,  

		"tags": ["falta", "angulo", "barreira"],  

		"activities": []  
	}],  

	"comments": [  
		{  
		"user_name": "José da Silva",  
		"text": "Lorem ipsum dolor sit amet, consectetur adipisicing elit",  
		"date_comment": null  
		}  
	]  
}   
```


## Retrieve - busca  

### Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.  

```  
be-mean-mongodb> db.projects.find({"name": /projeto 1/i})  
{  
  "_id": ObjectId("566f1913d0db00d58dd9aafe"),  
  "name": "Projeto 1",  
  "description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit",  
  "date_begin": null,  
  "date_dream": null,  
  "date_end": null,  
  "visible": false,  
  "realocate": false,  
  "expired": false,  
  "visualizable_mod": null,  
  "tags": [  
    "web",  
    "nfl",  
    "be mean"  
  ],  
  "members": [  
    {  
      "name": "Edson Arantes do Nascimento",  
      "type_member": "Tipo B",  
      "notify": false  
    },  
    {  
      "name": "Carlitos Tevez",  
      "type_member": "Tipo C",  
      "notify": false  
    },  
    {  
      "name": "Stephen Curry",  
      "type_member": "Tipo A",  
      "notify": false  
    },  
    {  
      "name": "Aaron Rodgers",  
      "type_member": "Tipo A",  
      "notify": false  
    },  
    {  
      "name": "Tom Brady",  
      "type_member": "Tipo A",  
      "notify": false  
    }  
  ],  
  "goals": [  
    {  
      "name": "Vagner Love",  
      "description": "Recebeu passe de Renato Augusto, goleiro adversário se adiantou e fez um golaço de cobertura!",  
      "date_begin": null,  
      "date_dream": null,  
      "date_end": null,  
      "realocate": false,  
      "expired": false,  
      "tags": [  
        "cobertura",  
        "chapeuzinho",  
        "por cima"  
      ],  
      "activities": [  
        {  
          "name": "Atividade 1",  
          "description": "Descrição completa da atividade 1",  
          "date_begin": null,  
          "date_dream": null,  
          "date_end": null,  
          "realocate": false,  
          "expired": false  
        },  
        {  
          "name": "Atividade 2",  
          "description": "Descrição completa da atividade 2",  
          "date_begin": null,  
          "date_dream": null,  
          "date_end": null,  
          "realocate": false,  
          "expired": false  
        }  
      ]  
    }  
  ],  
  "comments": [  
    {  
      "user_name": "Ednilson Amaral",  
      "text": "Belo gol de Vagner Love. Demorou para desencantar, mas depois que desencantou, não pára mais de fazer gol! Isso é ótimo para ele e perfeito para o Corinthians, parça!",  
      "date_comment": null  
    }  
  ]  
}  
Fetched 1 record(s) in 10ms  
```


### Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.  

```  
be-mean-mongodb> var query = {tags: {$in: [/web/i]}}  

be-mean-mongodb> db.projects.find(query, {name: 1})  
{  
  "_id": ObjectId("566f1913d0db00d58dd9aafe"),  
  "name": "Projeto 1"  
}  
{  
  "_id": ObjectId("566f1913d0db00d58dd9aaff"),  
  "name": "Projeto 2"  
}  
{  
  "_id": ObjectId("566f1913d0db00d58dd9ab00"),  
  "name": "Projeto 3"  
}  
Fetched 3 record(s) in 3ms  
```


### Liste apenas os nomes de todas as atividades para todos os projetos.  

```  
be-mean-mongodb> var query = {'name': 1, 'goals.activities.name': 1}  

be-mean-mongodb> query  
{  
  "name": 1,  
  "goals.activities.name": 1  
}  

be-mean-mongodb> db.projects.find({}, query)  
{  
  "_id": ObjectId("566f1913d0db00d58dd9aafe"),  
  "name": "Projeto 1",  
  "goals": [  
    {  
      "activities": [  
        {  
          "name": "Atividade 1"  
        },  
        {  
          "name": "Atividade 2"  
        }  
      ]  
    }  
  ]  
}  
{  
  "_id": ObjectId("566f1913d0db00d58dd9aaff"),  
  "name": "Projeto 2",  
  "goals": [  
    {  
      "activities": [  
        {  
          "name": "Atividade 1"  
        },  
        {  
          "name": "Atividade 2"  
        }  
      ]  
    }  
  ]  
}  
{  
  "_id": ObjectId("566f1913d0db00d58dd9ab00"),  
  "name": "Projeto 3",  
  "goals": [  
    {  
      "activities": [  
        {  
          "name": "Atividade 1"  
        },  
        {  
          "name": "Atividade 2"  
        }  
      ]  
    }  
  ]  
}  
{  
  "_id": ObjectId("566f1913d0db00d58dd9ab01"),  
  "name": "Projeto 5",  
  "goals": [  
    {  
      "activities": [ ]  
    }  
  ]  
}  
{  
  "_id": ObjectId("566f1913d0db00d58dd9ab02"),  
  "name": "Projeto 4",  
  "goals": [  
    {  
      "activities": [  
        {  
          "name": "Atividade 1"  
        },  
        {  
          "name": "Atividade 2"  
        }  
      ]  
    }  
  ]  
}  
Fetched 5 record(s) in 5ms  
```


### Liste todos os projetos que não possuam uma tag.  

```  
be-mean-mongodb> db.projects.find({tags: {$size: 0}})  
Fetched 0 record(s) in 1ms  
```  

Pelo o que li na documentação oficial do MongoDB, na parte que explica o `$size`, ele vai retornar a quantidade de argumentos que estejam dentro de um array. Tipo se coloco `$size: 1` ele vai mostrar os objetos que possuam um argumento no campo especificado. Se eu entendi corretamente, então, `$size: 0` vai retornar os projetos que não possua nenhuma `tag`. Caso eu tenha entendido errado, sorry. :s


### Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.  

```  
be-mean-mongodb> db.projects.find({}, {'members.name': 1}).skip(1)  
{  
  "_id": ObjectId("566f1913d0db00d58dd9aaff"),  
  "members": [  
    {  
      "name": "GPP"  
    },  
    {  
      "name": "Odel Bechkam Jr"  
    },  
    {  
      "name": "Victor Cruz"  
    },  
    {  
      "name": "Bill Bellicheck"  
    },  
    {  
      "name": "Luis Enrique"  
    }  
  ]  
}  
{  
  "_id": ObjectId("566f1913d0db00d58dd9ab00"),  
  "members": [  
    {  
      "name": "PVC"  
    },  
    {  
      "name": "Everaldo Marques"  
    },  
    {  
      "name": "Carlão Barreto"  
    },  
    {  
      "name": "Mauro Betting"  
    },  
    {  
      "name": "Nivaldo Pietro"  
    }  
  ]  
}  
{  
  "_id": ObjectId("566f1913d0db00d58dd9ab01"),  
  "members": [  
    {  
      "name": "Ronaldo"  
    },  
    {  
      "name": "Milton Leite"  
    },  
    {  
      "name": "Galvão Bueno"  
    },  
    {  
      "name": "Eli Manning"  
    },  
    {  
      "name": "Peyton Manning"  
    }  
  ]  
}  
{  
  "_id": ObjectId("566f1913d0db00d58dd9ab02"),  
  "members": [  
    {  
      "name": "Juninho Pernambucano"  
    },  
    {  
      "name": "William"  
    },  
    {  
      "name": "Junior"  
    },  
    {  
      "name": "Luis Roberto"  
    },  
    {  
      "name": "Cleber Machado"  
    }  
  ]  
}  
Fetched 4 record(s) in 7ms  
```


## Update - alteração  

### Adicione para todos os projetos o campo views: 0.
### Adicione 1 tag diferente para cada projeto.
### Adicione 2 membros diferentes para cada projeto.
### Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.
### Adicione 1 projeto inteiro com UPSERT.

## Delete - remoção  

### Apague todos os projetos que não possuam tags.
### Apague todos os projetos que não possuam comentários nas atividades.
### Apague todos os projetos que não possuam atividades.
### Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.
### Apague todos os projetos que possuam uma determinada tag em goal.


## Sharding
// coloque aqui todos os comandos que você executou


## Replica
// coloque aqui todos os comandos que você executou