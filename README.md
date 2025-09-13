# M87 e Pianeti e Stelle
![Immagine buco nero](./Screenshot%202025-04-09%20172131.png)

Mi sono appassionato ai buchi neri dopo aver letto due libri che mi hanno completamente catturato: "Buchi neri e salti temporali " di Kip Thorne, e "Luce dal nulla, L’immagine del buco nero e la nascita dell’astronomia del futuro" di Heino Falcke. Queste letture mi hanno aperto gli occhi su quanto la fisica dei buchi neri sia affascinante, ma anche profondamente legata alla nostra capacità di osservare l’universo in modi nuovi.

Kip Thorne è un fisico teorico statunitense, vincitore del Premio Nobel per la Fisica nel 2017 per il suo lavoro sulle onde gravitazionali con LIGO, e consulente scientifico per il film Interstellar. Con uno stile chiaro e immaginifico, ha saputo raccontare concetti estremamente complessi come i wormhole, la relatività generale e l’orizzonte degli eventi, rendendoli accessibili anche ai non addetti ai lavori.

Heino Falcke, astrofisico tedesco, è invece uno dei protagonisti reali della prima immagine di un buco nero mai ottenuta. È stato lui, infatti, a proporre per primo la possibilità di osservare un “ombra” del buco nero utilizzando una rete di radiotelescopi sulla Terra: una visione che si è concretizzata nel 2019 con l’immagine del buco nero al centro della galassia M87, ottenuta grazie alla collaborazione internazionale Event Horizon Telescope (EHT).

Proprio questa immagine ha ispirato il mio progetto: un tentativo di ricostruire l’immagine del buco nero M87 partendo dai dati osservativi, attraverso uno script didattico basato sul pacchetto eht-imaging (ehtim). Il notebook incluso, ehtim_tutorial_m87.ipynb, guida passo passo nell’elaborazione e nell’imaging dei dati interferometrici che simulano quelli raccolti dai radiotelescopi del progetto EHT.

Per eseguire il progetto è necessario installare alcune librerie scientifiche fondamentali come numpy, astropy, matplotlib e, ovviamente, ehtim. Considerando la complessità dell’ambiente e la presenza di librerie specifiche, è fortemente consigliato l’utilizzo di Docker: in questo modo si può lanciare un contenitore già configurato con tutte le dipendenze necessarie, garantendo un ambiente riproducibile e stabile.

Il mio obiettivo non è solo tecnico, ma anche divulgativo: mostrare come, con strumenti open source e dati pubblici, sia possibile riavvicinarsi a una delle più grandi imprese scientifiche degli ultimi decenni, e farlo in modo trasparente, accessibile e ripetibile.

---

# MY WORK M87 - Ricostruzione Immagine Radioastronomica di M87 da dati UVFITS
![Testo alternativo](/M87.png)
## 🔎 Descrizione generale
Questo notebook esegue la **ricostruzione di un’immagine astronomica reale** del buco nero di M87 a partire da dati interferometrici contenuti in un file **UVFITS**.  
I dati derivano da osservazioni VLBI (Very Long Baseline Interferometry), dove più radiotelescopi distribuiti sulla Terra osservano simultaneamente la stessa sorgente celeste. Vedi la cartella /MY_WORK_M87. L’obiettivo è passare dal **dominio delle visibilità** (piano u-v, ossia trasformata di Fourier dei segnali ricevuti) alla **ricostruzione spaziale** (immagine dell’oggetto celeste).

## ⚙️ Workflow
1. **Caricamento dati UVFITS**  
   - Utilizza `astropy.io.fits` per aprire il file `hops_lo_3601_M87+zbl-dtcal_selfcal.uvfits`.  
   - Estrae le colonne `UU---SIN`, `VV---SIN` (coordinate spaziali normalizzate) e `DATA` (visibilità complesse: Re, Im, Peso).

2. **Pre-elaborazione delle visibilità**
   - Separazione delle componenti reali e immaginarie.  
   - Media pesata sui canali, sfruttando i pesi (`WEIGHT`) per aumentare la robustezza.  
   - Pulizia dei dati: rimozione di valori NaN o con peso nullo.

3. **Filtraggio delle baseline**
   - Conversione delle coordinate u,v in unità di lunghezza d’onda (`lambda`).  
   - Eliminazione delle baseline corte (< 5 Mλ), che tendono ad aggiungere rumore e artefatti all’immagine.

4. **Interpolazione sul piano u-v**
   - Grigliatura delle visibilità su una mesh regolare con `scipy.interpolate.griddata`.  
   - Scelta del metodo lineare con riempimento dei valori mancanti a 0.

5. **Trasformata di Fourier inversa (IFFT)**
   - Applicazione della trasformata inversa 2D per ottenere l’immagine nel dominio spaziale.  
   - Shift delle componenti con `fftshift` per una corretta centratura.  
   - Normalizzazione dell’intensità.

6. **Upscaling e visualizzazione**
   - Uso di interpolazione bicubica (`scipy.ndimage.zoom`) per aumentare la risoluzione visiva.  
   - Visualizzazione con `matplotlib` e colormap **inferno**, adatta per imaging astronomico.

## 📦 Librerie utilizzate
- **[Astropy](https://www.astropy.org/)** → lettura file FITS/UVFITS.  
- **[NumPy](https://numpy.org/)** → gestione array numerici e operazioni vettoriali.  
- **[SciPy](https://scipy.org/)** → interpolazione (`griddata`), trasformate di Fourier, zoom bicubico.  
- **[Matplotlib](https://matplotlib.org/)** → grafici e visualizzazione immagini.

## 🔬 Metodologie e approcci
- **Interferometria radio**: le visibilità complesse rappresentano i campioni della trasformata di Fourier dell’immagine celeste.  
- **Media pesata**: migliora il rapporto segnale/rumore integrando correttamente più canali.  
- **Filtraggio baseline**: elimina contributi indesiderati da misure a bassa risoluzione angolare.  
- **Interpolazione su griglia**: necessaria per applicare la IFFT, dato che i dati osservativi sono irregolarmente campionati.  
- **FFT inversa**: permette di passare dal piano u-v (Fourier) al piano immagine.  
- **Upscaling bicubico**: utile per la presentazione, senza introdurre nuova informazione scientifica.  

## 📊 Output attesi
- Range delle baseline in unità di lunghezza d’onda.  
- Visualizzazione del piano u-v interpolato.  
- Ricostruzione dell’immagine di M87 (scala logaritmica consigliata per contrastare le forti dinamiche).  
- Zoom ad alta risoluzione per evidenziare i dettagli.  

## 🚀 Possibili estensioni
- Applicazione di algoritmi avanzati di **deconvoluzione CLEAN**.  
- Ricostruzione con **algoritmi regolarizzati** (es. Maximum Entropy, RML imaging).  
- Analisi comparativa con altre frequenze / dataset.  
- Automazione della pipeline per più UVFITS.  

---

# Pianeti_Stelle-Informazioni generali 
Si classificano stelle e pianeti nel tentativo di trovare tecniche di analisi di database specifici. In futuro si implementa una rete neurale per l'estrazione delle features e addestramento.
Come hanno realizzare la prima immagine del buco nero M87? 

Per realizzare la prima immagine del buco nero M87, gli scienziati hanno utilizzato tecniche avanzate di calcolo e imaging attraverso il progetto Event Horizon Telescope (EHT). Il metodo chiave impiegato è stato la interferometria a lunghissima base (VLBI), che sincronizza i segnali raccolti da telescopi posizionati in diverse parti del mondo, creando un unico telescopio virtuale delle dimensioni della Terra. Questa tecnica ha permesso di ottenere una risoluzione angolare estremamente elevata, necessaria per catturare l'ombra del buco nero.

Uno dei problemi principali era gestire la grande quantità di dati raccolti. Ogni telescopio del network generava enormi volumi di informazioni, circa 5 petabyte in soli quattro giorni di osservazioni. Poiché la trasmissione via internet non era sufficiente a gestire tali quantità, i dati sono stati fisicamente trasportati in appositi supercomputer situati al Max Planck Institute e all'Osservatorio Haystack del MIT per essere elaborati. La sincronizzazione delle osservazioni è stata garantita tramite orologi atomici, che permettevano di timbrare i dati con una precisione estrema.

L'elaborazione delle immagini è stata realizzata attraverso sofisticati algoritmi di imaging. Tra questi, spicca il pacchetto eht-imaging, sviluppato in Python, che ha sfruttato librerie scientifiche come NumPy e Astropy. Questi strumenti hanno permesso di combinare i dati provenienti dai vari telescopi e di generare l'immagine finale, riempiendo eventuali "vuoti" nelle osservazioni.


1. **NumPy (Numerical Python)**:  
   È una libreria che fornisce supporto per array multidimensionali (come matrici), insieme a una vasta gamma di funzioni matematiche e operazioni su questi array. NumPy è la base di molti pacchetti scientifici in Python grazie alla sua efficienza nel manipolare grandi volumi di dati numerici.

2. **Astropy**:  
   È una libreria Python sviluppata specificamente per l'astronomia. Fornisce strumenti per gestire unità fisiche, coordinate celesti, dati astronomici, e molto altro. È ampiamente usata nella comunità astronomica per elaborare e analizzare dati osservativi.

   Esempio di utilizzo di Astropy per lavorare con coordinate celesti:
   ```python
   from astropy.coordinates import SkyCoord
   from astropy import units as u
   
   # Creare una coordinata celeste (in coordinate equatoriali)
   coord = SkyCoord(ra=10.684*u.degree, dec=41.269*u.degree, frame='icrs')
   
   # Stampare le coordinate
   print("Coordinate RA:", coord.ra)
   print("Coordinate DEC:", coord.dec)
   ```

In sintesi, **NumPy** è utilizzata per gestire i dati numerici e le operazioni matematiche, mentre **Astropy** offre strumenti specifici per l'astronomia, come la gestione delle coordinate e delle unità fisiche. Entrambe le librerie sono cruciali in progetti come **eht-imaging**, dove si devono analizzare enormi quantità di dati astronomici per ricostruire immagini complesse come quelle di un buco nero.

Le tecniche computazionali sono state fondamentali anche per affrontare sfide come le interferenze atmosferiche e la riduzione massiva dei dati, senza trascurare i modelli della relatività generale per confermare che l'immagine rappresentava effettivamente l'ombra del buco nero​(Event Horizon Telescope)​(NASA Jet Propulsion Laboratory (JPL)).

---------------------------------------------------------------------------------------

# Analisi Spettrale della Luce Riflessa: 

I pianeti riflettono la luce in determinate lunghezze d’onda che possono essere analizzate per identificare la composizione chimica. Ogni elemento chimico ha un "impronta digitale" nel suo spettro di assorbimento, visibile in immagini spettrografiche.

Per simulare questo con un'immagine normale, puoi analizzare i canali di colore (RGB o, preferibilmente, HSL o YUV) per identificare caratteristiche salienti della riflessione.

Elaborazione dell'Immagine (Feature Extraction): Usa librerie come OpenCV per l'elaborazione delle immagini. Si possono estrarre feature legate alla luminosità, alla distribuzione dei colori e alla texture, che saranno utili per l'algoritmo di classificazione.

---------------------------------------------------------------------------------------


# Spettroscopia Simulata (Opzionale):

 Se si desidera aggiungere un livello realistico, è possibile usare tecniche di spettroscopia simulata per mappare l'intensità della luce a varie lunghezze d’onda e associare queste informazioni con specifiche firme spettrali di materiali noti (come acqua, metano, ecc.). L’uso di strumenti come pandas e scikit-learn per modelli di regressione potrebbe essere utile per analizzare questi dati.

Dataset Astronomical Data in Python è un database utile nella pratica per lavorare con i dati astronomici. Gli argomenti trattati includono:

- Scrittura di query che selezionano e scaricano dati da un database.
- Utilizzando i dati memorizzati in un Astropy o Pandas .TableDataFrame
- Utilizzo di coordinate e altre quantità con le unità.
- Archiviazione dei dati in vari formati.
- Esecuzione di operazioni di join del database che combinano i dati di più tabelle.
- Visualizzazione dei dati e preparazione di dati sulla qualità delle pubblicazioni.

SITO: https://allendowney.github.io/AstronomicalData/README.html

---------------------------------------------------------------------------------------


Tutorial come importare i file .fits:

- Open FITS files and load image data
- Make a 2D histogram with image data
- Stack several images into a single image
- Write image data to a FITS file
Sito:https://learn.astropy.org/tutorials/FITS-images.html
Sito Hubble Archive: https://hla.stsci.edu/
Esa Archive Explorer: https://hst.esac.esa.int/hcv-explorer/ ---> cerca il pianete e scarica il file tif
NASA/HEASARC SkyView: 
This service offers FITS files and is easy to navigate. Just enter "M87" in the search box and you'll be able to download the FITS file from the results page. This can be accessed at NASA SkyView​-----> Ho scaricato M87 non corrotto e funzionante

---------------------------------------------------------------------------------------
TESI DI LAUREA: Classificazione_spettrale_delle_stelle

https://amslaurea.unibo.it/19482/1/Classificazione_spettrale_delle_stelle_Prandi_Riccardo.pdf

-----------------------------------------------------------------------------------------

# Dati Spettrali
Ecco una tabella dei dati spettrali degli elementi della tavola periodica che può esserti utile:

- Atomic Spectra: Un sito web, Atomic Spectra, fornisce informazioni dettagliate sulle spettri atomici degli elementi. Ogni elemento produce uno spettro unico quando viene eccitato, mostrando linee spettrali a specifiche lunghezze d'onda. La tabella interattiva presenta dati visivi e informazioni sulle lunghezze d'onda associate a ciascun elemento​(
Atomic Spectra
).

- WebElements: Un altro sito utile è WebElements, che offre una galleria di spettri atomici degli elementi. Qui puoi esplorare gli spettri di emissione e assorbimento, con collegamenti a immagini e dati sulle lunghezze d'onda. Ogni elemento è presentato con dettagli sui suoi spettri e la loro interpretazione​(
WebElements
).

- NIST Atomic Spectra Database: Il database NIST è una risorsa importante per dati spettroscopici, che raccoglie informazioni da varie fonti. Puoi accedere ai dati spettrali di ogni elemento, inclusi i livelli energetici e le lunghezze d'onda delle transizioni elettroniche.

-----------------------------------------------------------------------------------------
# Dataset utili: 

- https://openexoplanetcatalogue.com/ L'Open Exoplanet Catalogue è un catalogo di tutti i pianeti extrasolari scoperti. Si tratta di un nuovo tipo di database astronomico. È decentralizzato e completamente aperto. Accogliamo con favore contributi e correzioni sia da astronomi professionisti che dal pubblico in generale.

- STAR_DATASET_2024.csv; https://exoplanetarchive.ipac.caltech.edu/cgi-bin/TblView/nph-tblView?app=ExoTbls&config=STELLARHOSTS; Puoi filtrre direttamente su sito. 

- https://ned.ipac.caltech.edu/

- The Dark Energy Spectroscopic Instrument Early Data Release (DESI-EDR; DESI Collaboration, 2024) catalog has been integrated into 	NED,including positions, object types (galaxy, star, QSO) and redshift information for 1.85 million unique objects.254K new object links (pointers) were added from over 2K new references.2M sources from the literature were cross-identified with NED objects. 639K new objects were added into the database.2M new redshift measurements were added (mostly from DESI-EDR), including data for 1.7 million objects that previously had no redshift.
Current total database holdings are summarized here.

- Sloan Digital Sky Survey (SDSS)-> https://skyserver.sdss.org/dr18

- NasaExoplanetArchive --> https://pypi.org/project/exoplanet/

- How open Exoplanet Cataloge -->https://github.com/OpenExoplanetCatalogue/open_exoplanet_catalogue

- Example to Exoplanet to fits model  --> https://gallery.exoplanet.codes/

- SIMBAD è un altro database molto utile per la ricerca di oggetti astronomici, incluse stelle, galassie, e talvolta pianeti. Puoi cercare specifici oggetti o tipi di oggetti.

- Il Gaia Mission dell'ESA fornisce dati molto dettagliati sulla posizione e sulle caratteristiche di molte stelle. 

- Il TESS è un'altra missione NASA dedicata alla scoperta di esopianeti attraverso il metodo del transito.

-------------------------------------------------------------------------------------------

# Classificazione in base allo Redshift delle stelle: 

Definire prima cosa è lo Redshift , la formula , poi capire come utilizzarlo per un eventuale classificazione. Trova un database valido , tipo della Nasa , ovviamente. Sito Download  DESI-EDR to Redshift : 

https://help.desi.lbl.gov/index.php?qa=8&qa_1=accessing-desi-edr-redshift-and-spectra-using-list-of-and-dec , devi inscriverti su troppi siti del cazzoo.
desihub--> sito ufficile per poter trovare materiale
specpod-db--> https://github.com/desihub/specprod-db
doc official --> https://specprod-db.readthedocs.io/_/downloads/en/stable/pdf/

oss: Prima di lavorare con il db esegui le seguenti operazioni:

1.Installa Jupiter notebook , per lavorare direttamente con i file locali
2.Configura l'enviroment chiamato env, questo contiene le librerie , può essere legato a solo un progetto o a tutti. Molto simili all'env di Anaconda. Solo che 	bisogna stare attenti nel configurazione!!
3.Scarica Postgres , per il progetto l'unico modo è usare questo gestore per il database. 
4. Installa librerie particolari per il progetto che servono per collegarti al   database PostgreSQL direttamente da JupyterLab con Python, utilizzando *sqlalchemy*  	*psycopg2*.o simile).
5. git clone  https://github.com/desihub/specprod-db
6. python setup.py install OPPURE pip install .---> settaggio necessario se no non riesce ad utilizzare moduli che sono contenuti nella cartella del progetto colonato 
7. Scarica Postgres , per il progetto l'unico modo è usare questo gestore per il database. 
8. L'Avvio e automatico di Postgres,ma meglio controllare. Non devi creare nessun database ma i codici indicati di sotto posso essere utili 
- psql -U your_username ->controlla quali db sono stati creati dall'utente username
- \l
- createdb -U your_username specprod-db
- db_url = 'postgresql://your_username:your_password@localhost:5432/specprod-db'
- sudo systemctl status postgresql
- sudo systemctl start postgresql
- netstat -an | grep 5432

Premi Win + R, digita "services.msc" e premi Invio.
Trova il Drive Service PostgreSQL (es. postgresql-x64-15.8-2 o simile).
Verifica che il servizio sia in esecuzione. In caso contrario, fai clic con il 	pulsante destro del mouse e seleziona "Start".

