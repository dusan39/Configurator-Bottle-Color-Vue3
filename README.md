# Dna analytics vue 3
 
Questo progetto è stato realizzato con l'utilizzo di: 

> <img src="https://img.icons8.com/external-tal-revivo-color-tal-revivo/24/null/external-vuejs-an-open-source-javascript-framework-for-building-user-interfaces-and-single-page-applications-logo-color-tal-revivo.png"/> Vue 3

Le librerie esterne utilizzate per questo progetto sono:

> Axios

> Vue-chartJs

![GitHub last commit](https://img.shields.io/github/last-commit/dusan39/Dna-Information-Vue3)

## Cosa troverete in questo file

- Link depoly dell'app con netlify
- Funzionalità
- Struttura del progetto
- Logiche del progetto

## Link deploy dell'app con netlify

Link: https://dnainformation.netlify.app/

## Funzionalità

<img width="1439" alt="Screenshot 2023-05-01 alle 16 23 10" src="https://user-images.githubusercontent.com/114413164/235466492-b3cce0e6-29d8-4170-be29-1d9a62be7774.png">

Questo progetto si basa su una chiamata API che fornisce diversi dati che tramite la libreria esterna vue-chartjs permette di mostrare su un grafico i dati ottenuti.
In questo caso otteniamo 4 oggetti diversi che mostreremo a video in base al grafico richiesto tramite il bottone che si trova sotto il grafico.

## Struttura del progetto

<img width="230" alt="Screenshot 2023-05-13 alle 12 56 30" src="https://github.com/dusan39/Dna-Information-Vue3/assets/114413164/dbb1aef2-aeb9-494d-b27d-f22cb357adec">


Il progetto si sviluppa su due componenti principali ovvero Chart.vue dove avviene tutta la manipolazione dei dati e creazione dei charts, invece nel componente Controller.vue viene utilizzato per creare i 4 controller che permettono il cambio dei dati da far visualizzare a video.

## Logiche del progetto

Questo progetto è stato realizzato in maniera ben precisa e strutturato in tanti "piccoli" passaggi tutto questo partendo da una singola chiamata di una funzione che si propaga con la gestione dei dati, la creazione dei charts e la loro visualizzazione.

Come prima cosa si cerca di ottenere i dati dalla chiamata API con la funzione asincrona getAllData(), che ho gestito in un file esterno ai componenti ovvero api.js, che ho riportato qui sotto:

```JavaScript
  async function getAllData(){
    try{
      const response = await axios.get(VITE_API_CHARTS)
      return response
    }catch(error){
      console.error(error)
    }      
  }
```

dopo aver ottenuto la risposta, se tutto è andato bene, la funzione viene esportata nei due componenti per ottenere i dati necessari ovvero uno per ottenere il totale delle 4 categorie invece nell'altro per creare i charts:


```JavaScript
  async function getSubscriptionsData(response){
    const subscriptions = response.data.subscriptions
    const subscriptionsKey = Object.keys(subscriptions.history)
    const subscriptionsValues = Object.values(subscriptions.history)

    subscriptionsTotal = subscriptions.total
    subscriptionsLabels = subscriptionsKey
    subscriptionsData = subscriptionsValues

    await createChartSubscriptions()
  }
```

La Funzione riportata qua sopra è la medesima delle restanti, questa si occupa di ottenere i dati dall'oggetto subscriptions e chiamare la funzione per la creazione del grafico:

```JavaScript
  async function createChartSubscriptions(){
    const data = {    
      labels: subscriptionsLabels,
      datasets: [{
        label: 'Subscriptions',
        backgroundColor: '#19A7CE',
        borderColor: '#19A7CE',
        data: subscriptionsData,
      }]
    };

    const config = {
      type: 'line',
      data: data,
      options: {}
    };

   const myChartSubscriptions = new Chart(document.getElementById('myChartSubscriptions'), config);
  }
```

tutte queste funzioni vengono gestite da una singola chiamata, al caricamento del DOM, viene chiamata la seguente funzione:

```JavaScript
  async function getDataAndCallFunctions() {
    try{
      const response = await getAllData()
      getSubscriptionsData(response)
      getImpressionsData(response)
      getClicksData(response)
      getAvgTimeData(response)
    }catch(error){
      console.error(error)
    }
  }
```

Come funzionalità extra ho aggiunto che il valore impressionsTotal venga incrementato di 5 ogni 2 secondi tramite questa funzione:

```JavaScript
  function startIncrementingImpressions() {
    setInterval(() => {
      data.impressionsTotal += 5;
    }, 2000);
  }
```

