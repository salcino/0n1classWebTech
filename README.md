# 0n1Cla$$ Web Tech

Boilerplate Node.js Vue.js Express Sequelize MySQL Bootstrap

In questo progetto, mostrerò come costruire un'applicazione completa (full-stack Vue.js + Node.js + Express + MySQL) con esempi e CRUD.
Il server di back-end utilizza Node.js ed Express per le REST APIs mentre il front-end è un client router Vue con axios.

Le tecnologie che utilizzeremo sono indicate di seguito.

| Tech        | Version     | Layer       | Link        |
| ----------- | ----------- | ----------- | ----------- |
|Node.js      |18.9.0       |Backend      |<https://nodejs.org/en/about>|
|Vue.js       |3.2.39       |Frontend     |<https://vuejs.org>|
|axios        |0.27.2       |http client  |<https://axios-http.com>|
|Express      |4.18.1       |Frontend     |<https://expressjs.com>|
|Sequelize    |6            |ORM          |<https://sequelize.org>|
|TypeScript   |4.8.3        |JavaScript   |<https://www.typescriptlang.org>|
|MySQL        |8.0.30       |Database     |<https://www.mysql.com/it>||
|Font Awesome |6.2.0        |Icons        |<https://fontawesome.com>|
|HeidiSQL     |12.1         |DB interface |<https://www.heidisql.com>|
|VS Code      |1.71         |IDE          |<https://code.visualstudio.com>|
|Atom         |1.60.0       |Editor       |<https://atom.io>|
|Notepad++    |8.4.5        |Editor       |<https://notepad-plus-plus.org>|
|git          |2.37.3       |Source Ctrl  |<https://git-scm.com>|
|GitHub       |-            |Source Arc.  |<https://github.com>|
|NuxtJS       |2.15.8       |Vue Framework|<https://nuxtjs.org>|
|Volar        |0.40.13      |VS Code Ext  |<https://marketplace.visualstudio.com/items?itemName=Vue.volar>|
|Inkscape     |1.1          |Editor Graph |<https://inkscape.org/it>|
|paint.net    |4.3.12       |Editor Graph |<https://www.getpaint.net>|

Costruiremo un'applicazione web full-stack per pubblicare online dei tutorial dove:

- Il tutorial è descritto da id, titolo, descrizione e stato di pubblicazione;
- L'utente può creare, richiedere, aggiornare o cancellare dei tutorial;
- Sarà presente una barra di ricerca per trovare i tutorial per titolo.

In sintesi saranno realizzate delle pagine applicative per aggiungere un nuovo tutorial, elencare tutti i tutorial presenti, editare un tutorial esistente (cambiare lo stato di pubblicazione, cancellare il tutorial, aggiornare i dati), ricercare i tutorial per titolo.

## Architettura

L'architettura che utilizzeremo è la seguente:
![Architettura del sistema](./docs/images/architettura_01.png)

- Node.js con Express espone le API REST e interagisce con il database MySQL utilizzando come ORM Sequelize;
- Vue Client invia richieste HTTP e recupera risposte HTTP utilizzando Axios, consuma dati sui componenti.
- Vue Router viene utilizzato per navigare tra le pagine.

## Start Up

Installazione di Node.js.

Installazione di Visual Studio Code.

Installazione di git.

Installazione di Notepad++.

Installazione di Inkscape.

Installazione di paint.net.

## Back-End (Node.js + Express)

### APIs

Di seguito l'elenco di API che esporremo:

| Metodi      | URLs                     | Azioni                   |
| ----------- | -----------              | ---------                |
|GET          |api/tutorials             |prendi tutti i tutorial   |
|GET          |api/tutorials/:id         |prendi tutorial per id    |
|POST         |api/tutorials             |aggiungi nuovo tutorial   |
|PUT          |api/tutorials/:id         |aggiorna tutorial per id  |
|DELETE       |api/tutorials/:id         |elimina tutorial per id   |
|DELETE       |api/tutorials             |elimina tutti i tutorial  |
|GET          |api/tutorials?title=[web] |trova tutti i tutorial il cui titolo contiene 'web'|

### Struttura del progetto

- db.config.js contiene i parametri di configurazione per la connessione a MySQL e Sequelize.
- Server Web Express in server.js dove configuriamo CORS, inizializziamo ed eseguiamo le API REST Express.
- Successivamente, aggiungiamo la configurazione per il database MySQL in models/index.js e configuriamo Sequelize data model in models/tutorial.model.js.
- I controller degli oggetti tutorial sono nella cartella controllers.
- In tutorial.routes.js ci sono le rotte per la gestione di tutte le operazioni CRUD.

## Implementazione

### Creazione del progetto su GitHub

Creiamo il progetto su GitHub e dopo scarichiamolo con Visual Studio Code.

### Creazione dell'applicazione Node.js

Come prima cosa creiamo la cartella 0n1classWebTech.

``` bash
> mkdir 0n1classWebTech
> cd 0n1classWebTech
```

Dopodiché inizializziamo l'applicazione Node.js creando il file package.json tramite il comando npm init:

``` text
npm init
name: (0n1classWebTech) 
version: (1.0.0) 
description: Boilerplate Node.js Vue.js Express Sequelize MySQL Bootstrap.
entry point: index.js
test command: 
git repository: 
keywords: nodejs, express, sequelize, mysql, rest, api
author: SLS
license: (ISC)

Is this ok? (yes) yes
```

Dobbiamo installare i moduli necessari: express, sequelize, mysql2 e body-parser. Di seguito i comandi per l'installazione dei moduli:

``` text
npm install express sequelize mysql2 body-parser cors –save
added 93 packages, and audited 94 packages in 12s

8 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
```

## Configurazione del web server Express

La prima cosa da fare è creare un file di configurazione che per convenzione si deve chiamare server.js, il file deve avere il seguente contenuto:

``` javascript
const express = require("express");
const bodyParser = require("body-parser");
const cors = require("cors");

const app = express();

var corsOptions = {
  origin: "http://localhost:8081"
};
app.use(cors(corsOptions));
// parse requests of content-type - application/json
app.use(bodyParser.json());
// parse requests of content-type - application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ extended: true }));
// simple route
app.get("/", (req, res) => {
  res.json({ message: "Benvenuto nell'applicazione Vue." });
});
// set port, listen for requests
const PORT = process.env.PORT || 8080;
app.listen(PORT, () => {
  console.log(`Server in esecuzione sulla porta ${PORT}.`);
});
```

Cosa dobbiamo fare adesso:

- Importare i moduli express, body-parser e CORS:
  - Express serve a costruire le API REST;
  - body-parser aiuta a "parsare" la richiesta ed a creare l'oggetto req.body;
  - cors fornisce un middleware Express per abilitare CORS con varie opzioni;
- Creare un'applicazione Express, aggiungere body-parser ed il middleware cors usando il metodo app.use(). Notare che è stata impostata come sorgente: <http://localhost:8081>.
- definiamo una rotta GET GET solo a scopo di test per vedere se il server parte.
- ci mettiamo in ascolto sulla porta 8080 per le richieste in arrivo.

Ora lanciamo l'applicazione server con il comando: node server.js.  
Aprire il browser all'URL <http://localhost:8080>, verrà mostrato il seguente contenuto:

``` json
{"message":"Benvenuto nell'applicazione Vue."}
```

## Configurare MySQL e Sequelize

Nella cartella app creare una nuova sotto cartella config nella quale creare un nuovo file db.config.js con il seguente contenuto:

``` json
module.exports = {
  HOST: "localhost",
  USER: "root",
  PASSWORD: "123456",
  DB: "testdb",
  dialect: "mysql",
  pool: {
    max: 5,
    min: 0,
    acquire: 30000,
    idle: 10000
  }
};
```

I primi cinque parametri sono per la connessione a MySQL, la sezione pool è opzionale ed è utilizzata da Sequelize per la gestione del pool di connessioni:

- max: massimo numero di connessioni nel pool
- min: minimo numero di connessioni nel pool
- idle: tempo massimo, in millisecondi, che una connessione viene tenuta prima di essere rilasciata
- acquire: tempo massimo, in millisecondi, che la connessione nel pool proverà a riprendere la connessione prima di lanciare un'errore.

Per maggiori dettagli, visita la documentazione delle API per il costruttore di Sequelize <https://sequelize.org/master/class/lib/sequelize.js~Sequelize.html#instance-constructor-constructor>.

### Inizializzazione di Sequelize

Inizializzeremo Sequelize nella cartella app/models che conterrà il modello nel passaggio successivo.  
Creiamo app/models/index.js con il seguente contenuto:

``` javascript
const dbConfig = require("../config/db.config.js");

const Sequelize = require("sequelize");
const sequelize = new Sequelize(dbConfig.DB, dbConfig.USER, dbConfig.PASSWORD, {
  host: dbConfig.HOST,
  dialect: dbConfig.dialect,
  operatorsAliases: false,

  pool: {
    max: dbConfig.pool.max,
    min: dbConfig.pool.min,
    acquire: dbConfig.pool.acquire,
    idle: dbConfig.pool.idle
  }
});

const db = {};

db.Sequelize = Sequelize;
db.sequelize = sequelize;

db.tutorials = require("./tutorial.model.js")(sequelize, Sequelize);

module.exports = db;

```

Non dimenticarsi di chiamare il metodo sync() nel file server.js:

``` javascript
...
const app = express();
app.use(...);

const db = require("./app/models");
db.sequelize.sync();
...
