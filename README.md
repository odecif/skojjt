# Skojjt
## Närvarorapportering för scouter. 

*av Martin Green/Tynnereds scoutkår.*

https://skojjt.appspot.com  
Prova [skojjt.appspot.com](https://skojjt.appspot.com), säg till [martin@famgreen.se](mailto:martin@famgreen.se) för att få access.

Målet med skojjt är en enkel närvaroregistrering som kan används av alla på avdelning.
Alternativen verkar så undermåliga för vår verksamhet, så ett eget system var den bästa möjligheten.
Man ska kunna göra sin registering direkt när man har mötet.
Det finns en direktkoppling till vårt eget medlemsregister, scoutnet. Vi kan synkronisera nya medlemmar direkt från scoutnet med en knapptryckning.
Det är en web-site som fungerar i mobiltelefon, inget behov av en app. Det ser ut som en app i telefonens browser.
Den hostas på Google app engine. Vilket ger följande fördelar:
* Google står för säkerheten. Användarna loggar in med sina google konton. Administratören sätter access i skojjt, sen kan dom registrera.
* Google står för SSL certifikatet. All trafik går via https.
* Driftsäkerheten är god.
* Det är gratis upp till en viss gräns för trafik och datamängd.
* Om det skulle bli många användare så klarar googles servrar det.

Det finns rapportering av närvaro per grupp (avdelning) som Göteborgs kommun kräver.
Vi har även möjlighet att koppla denna närvaro till andra partners, t ex Sensus studieförbund.

Skojjt implementerar APN/DAK för redovisning till Göteborgs kommun:
 * http://www.sverigesforeningssystem.se/dak-formatet/vad-ar-dak/
 * http://ukf.umea.se/aktivitetskort

### [Dokumentation](https://github.com/Scouterna/skojjt/wiki)

### Hur man testar/utvecklar i Windows:
* Installera Git
* Installera [Google Cloud SDK](https://dl.google.com/dl/cloudsdk/channels/rapid/GoogleCloudSDKInstaller.exe).
* Python 2 följer med Google Cloud SDK, så vi behöver inte installera Python.
* Initiera Google Cloud:
    + `gcloud init`
* Uppdatera till en version som fungerar:
    + `gcloud components update --version 341.0.0`
* Installera python komponenter för gcloud:
    + `gcloud components install app-engine-python-extras`
* Klona git-repot: 
    + `git clone https://github.com/Scouterna/skojjt`
* Uppdatera biblioteken (kan även behövas efter pull):
    + `scripts\update_libs.bat`
* Du kan behöva uppdatera Google Cloud (om du redan har en installation):
    + `gcloud components update`
* Om du gör ändringar i data eller queries så kan du behöva köra:
    + `gcloud datastore indexes create index.yaml`
* Starta servern lokalt:
    + `scripts\run_local.bat`
* Öppna en webläsare på adress: [http://localhost:8080/](http://localhost:8080/)
* Deploy projektet publikt som `skojjt-X`(ditt val av namn)
  + `gcloud app deploy index.yaml --project skojjt-X`
  + `gcloud app deploy app.yaml --project skojjt-X`
* Testa appen `gcloud app browse`

### Hur man testar/utvecklar i Linux:
* Klona git-repon till lokal dator.
* Installera Python 2.7 och [Google App Engine SDK (GAE)](https://cloud.google.com/sdk/docs/#deb).
* Installera biblioteken. `pip install -r requirements.txt -t lib`
* Konfigurera GAE `gcloud init` och följ instruktionerna.
* Deploy kod `gcloud app deploy` från git mappen
* Update indexes`gcloud datastore indexes create index.yaml`
* Testa appen `gcloud app browse`

### Hur man testar/utvecklar på Mac:
* Använd homebrew för att installera Python 2.7 och Google App Engine.
    + Installera homebrew om inte redan gjort 
    + Installera python2 i homebrew
    + Installera Google App Engine (ligger i en cask eftersom den är binär)
        - `brew cask info google-cloud-sdk`
        - `brew tap caskroom/cask`
        - `brew cask install google-cloud-sdk`
        - `source /usr/local/Caskroom/google-cloud-sdk/latest/google-cloud-sdk/path.bash.inc`
        - `source /usr/local/Caskroom/google-cloud-sdk/latest/google-cloud-sdk/completion.bash.inc`
* Konfigurera GAE `gcloud init`
* Kör lokal test-server och admin fönster:
  + `dev_appserver.py app.yaml`
  + `open http://localhost:8080`
* Deploy projektet `skojjt-X`(ditt val av namn)
  + `gcloud app deploy index.yaml --project skojjt-X`
  + `gcloud app deploy app.yaml --project skojjt-X`
* Testa appen `gcloud app browse`

### Continuous deployment

Senaste versionen från grenen `GAE/master` finns tillgänglig på
https://skojjt.appengine-staging.scouterna.net för testning.
