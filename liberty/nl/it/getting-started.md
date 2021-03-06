---

copyright:
  years: 2017, 2018
lastupdated: "2018-02-14"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:app_name: data-hd-keyref="app_name"}

# Esercitazione introduttiva

* {: download} Congratulazioni, hai distribuito un'applicazione di esempio Hello World su {{site.data.keyword.Bluemix}}!  Per iniziare, segui questa guida dettagliata. O <a class="xref" href="http://bluemix.net" target="_blank" title="(Scarica il codice di esempio)"><img class="hidden" src="../../images/btn_starter-code.svg" alt="Scarica codice di esempio" />scarica codice di esempio</a> o esplora da solo.

Seguendo questa esercitazione introduttiva Liberty for Java, configurerai un ambiente di sviluppo, distribuirai un'applicazione localmente e in {{site.data.keyword.Bluemix}} e integrerai un servizio database nella tua applicazione. 

## Prima di cominciare
{: #prereqs}

Ti serviranno i seguenti account e strumenti:
* [Account {{site.data.keyword.Bluemix_notm}}](https://console.ng.bluemix.net/registration/)
* [CLI Cloud Foundry ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/cloudfoundry/cli#downloads){: new_window}
* [Git ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://git-scm.com/downloads){: new_window}
* [Maven ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://maven.apache.org/download.cgi){: new_window}

## Passo 1: Clona l'applicazione di esempio
{: #clone}

Per prima cosa, clona il repository GitHub dell'applicazione di esempio.
  ```bash
git clone https://github.com/IBM-Bluemix/get-started-java
  ```
  {: pre}


## Passo 2: Esegui l'applicazione localmente utilizzando la riga di comando
{: #run_locally}

Utilizza Maven per creare il tuo codice di origine ed eseguire l'applicazione risultante.

1. Nella riga di comando, modifica la directory in cui è ubicata l'applicazione di esempio.

  ```
cd get-started-java
  ```
  {: pre}

1. Utilizza Maven per installare le dipendenze e creare il file .war.

  ```
mvn clean install
  ```
  {: pre}

1. Esegui l'applicazione localmente su Liberty.
  ```
mvn install liberty:run-server
  ```
  {: pre}

Quando visualizzi il messaggio *The server defaultServer is ready to run a smarter planet.*, puoi visualizzare la tua applicazione all'indirizzo: http://localhost:9080/GetStartedJava.

Per arrestare la tua applicazione, premi *Ctrl-C* nella finestra della riga di comando in cui hai avviato l'applicazione.

## Passo 3: Prepara l'applicazione per la distribuzione
{: #prepare}

Per distribuire a {{site.data.keyword.Bluemix_notm}}, può essere utile impostare un file manifest.yml. Il manifest.yml include le informazioni di base sulla tua applicazione, come il nome, quanta memoria allocare per ogni istanza e la rotta. Abbiamo fornito un file manifest.yml di esempio nella directory `get-started-java`.

Apri il file manifest.yml e modifica `name` da `GetStartedJava` con il nome della tua applicazione, <var class="keyword varname" data-hd-keyref="app_name">app_name</var>.
{: download}

  ```
  applications:
   - name: GetStartedJava
     random-route: true
     path: target/GetStartedJava.war
     memory: 512M
     instances: 1
  ```
  {: codeblock}

In questo file manifest.yml, **random-route: true** genera una rotta casuale per la tua applicazione per evitare il conflitto con altre rotte.  Se scegli di farlo, puoi sostituire **random-route: true** con **host: myChosenHostName**, fornendo un nome host di tua scelta. [Ulteriori informazioni...](/docs/manageapps/depapps.html#appmanifest)
{: tip}

## Passo 4: Distribuisci a {{site.data.keyword.Bluemix_notm}}
{: #deploy}

Distribuisci la tua applicazione a una delle seguenti regioni {{site.data.keyword.Bluemix_notm}}. Per una latenza ottimale, scegli una regione il più vicina possibile ai tuoi utenti.

| **Nome regione** | **Ubicazione geografica** | **Endpoint API** |
|-----------------|-------------------------|-------------------|
| Regione Stati Uniti Sud | Dallas, US | api.ng.bluemix.net |
| Regione Stati Uniti Est | Washington, DC, US | api.us-east.bluemix.net |
| Regione Regno Unito | Londra, Inghilterra | api.eu-gb.bluemix.net |
| Regione Sydney | Sydney, Australia | api.au-syd.bluemix.net |
| Regione Germania | Francoforte, Germania | api.eu-de.bluemix.net |
{: caption="Tabella 1. Elenco di regioni {{site.data.keyword.cloud_notm}}" caption-side="top"}

1. Configura l'endpoint API sostituendo  `<API-endpoint>`  con l'endpoint per la tua regione.
  ```
cf api <API-endpoint>
  ```
  {: pre}

1. Accedi al tuo account {{site.data.keyword.Bluemix_notm}}.
  ```
cf login
  ```
  {: pre}

  Se non puoi accedere utilizzando i comandi `cf login` o `bx login` perché il tuo ID utente è federato, utilizza i comandi `cf login --sso` o `bx login --sso` con il tuo ID SSO (Single Sign On). Consulta [Accesso con un ID federato](https://console.bluemix.net/docs/cli/login_federated_id.html#federated_id) per ulteriori informazioni.


1. Dall'interno della directory `get-started-java`, trasmetti la tua applicazione a {{site.data.keyword.Bluemix_notm}}.
  ```
cf push
  ```
  {: pre}

La distribuzione della tua applicazione può impiegare diversi minuti. Quando la distribuzione è stata completata, visualizzerai un messaggio che la tua applicazione è in esecuzione. Visualizza la tua applicazione all'URL elencato nell'output del comando trasmesso o visualizza l'URL e lo stato di distribuzione dell'applicazione eseguendo il seguente comando:
  ```
cf apps
  ```
  {: pre}

Puoi risolvere gli errori nel processo di distribuzione eseguendo il comando `cf logs <Your-App-Name> --recent`.
{: tip}  

## Passo 5: Aggiungi un database
{: #add_database}

Successivamente, aggiungeremo un database NoSQL a questa applicazione e la configureremo in modo che possa essere eseguita localmente o su {{site.data.keyword.Bluemix_notm}}.

1. Nel tuo browser, accedi a {{site.data.keyword.Bluemix_notm}} e passa al dashboard. Seleziona **Create Resource**.
2. Scegli la sezione **Data and Analytics**, seleziona **Cloudant NoSQL DB** e crea il tuo servizio.
3. Passa alla vista **Connections** e seleziona la tua applicazione, quindi **Create connection**.
4. Seleziona **Restage** quando richiesto. {{site.data.keyword.Bluemix_notm}} riavvierà la tua applicazione e fornirà le credenziali del database alla tua applicazione utilizzando la variabile di ambiente `VCAP_SERVICES`. Questa variabile di ambiente è disponibile per l'applicazione solo quando è in esecuzione su {{site.data.keyword.Bluemix_notm}}.

Le variabili di ambiente ti abilitano a separare le impostazioni di distribuzione dal tuo codice di origine. Ad esempio, invece di impostare come hardcoded una password del database, puoi archiviarla in una variabile di ambiente di riferimento nel tuo codice di origine. [Ulteriori informazioni...](/docs/manageapps/depapps.html#app_env)
{: tip}

## Passo 7: Utilizza il database
{: #use_database}
Ora aggiorneremo il tuo codice locale per puntare a questo database. Archivieremo le credenziali per i servizi in un file properties. Questo file sarà utilizzato SOLO quando l'applicazione è in esecuzione localmente. Quando è in esecuzione in {{site.data.keyword.Bluemix_notm}}, le credenziali saranno lette dalla variabile di ambiente `VCAP_SERVICES`.

1. Nel tuo browser, vai a {{site.data.keyword.Bluemix_notm}} e seleziona **Apps > _ your app_ _ > Connections > Cloudant > View Credentials**.

2. Copia e incolla solo l'`url` dalle credenziali nel campo `url` del file `cloudant.properties` e salva le modifiche.
  ```
  cloudant_url=https://123456789 ... bluemix.cloudant.com
  ```
  {:pre}

3. Riavvia il server

  Aggiorna la tua vista del browser all'indirizzo http://localhost:9080/GetStartedJava/. Tutti i nomi immessi nell'applicazione saranno ora aggiunti al database.

  La tua applicazione locale e {{site.data.keyword.Bluemix_notm}} stanno condividendo il database. I nomi che aggiungi dall'applicazione saranno visualizzati entrambi quando aggiorni i browser.

Ricorda, se non hai bisogno della tua applicazione live su {{site.data.keyword.Bluemix_notm}}, arrestala così da non incorrere in alcun addebito non previsto.
{: tip}  

## Passi successivi

* [Esercitazioni](/docs/tutorials/index.html)
* [Samples ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://ibm-cloud.github.io){: new_window}
* [Architecture Center ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/cloud/garage/category/architectures){: new_window}
