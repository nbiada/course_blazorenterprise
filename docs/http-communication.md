# HTTPClient

## Blazor WebAssembly
L'_HttpClient_ è pronto per essere utilizzato con i settings di default.

È sufficiente effettuare un inject dell'interfaccia ed utilizzarlo direttamente.

## Blazor Server
_HttpClient_ è simile ma richiede alcune configurazioni aggiuntive.

## HttpClientFactory
A partire da .net Core 2.1 è disponibile una classe specifica, chiamata _HttpClientFactory_, che si occupa di istanziare HttpClient e gestisce le varie problematiche associate.

### Opzioni
_HttpClientFactory_ supporta diverse opzioni di configurazione:
- Basic clients
- Named clients
- Typed clients
- Generated clients

Al momento ci concentriamo sull'opzione __Typed clients__ che rappresenta l'opzione consigliata da Microsoft per le applicazioni enterprise.

## Configurazione
Nello scenario di base un'applicazione Blazor implementa un servizio HttpClient all'interno di `ConfigureServices`:

```csharp
services.AddScoped<HttpClient>(s => {
    var client = new HttpClient { BaseAddress = new Syste.Uri("https://api.dom") };
    return client;
})
```
Questo approccio non consente di richiamare Rest APIs con indirizzi di base differenti.

### Helper classes
Blazor mette a disposizione delle classi di Helper che facilitano e promuovono uno sviluppo più strutturato.

```csharp
var appURI = new Uri("http://app.dom");

void RegisterTypedClient<TClient, TImplementation>(Uri apiBaseUrl) where TClient: class where TImplementation: class, TClient {
    services.AddHttpClient<TClient, TImplementation>(client =>
    {
        client.BaseAddress = apiBaseUrl;
    });
}

// Http Services
RegisterTypedClient<IEmployeeDataService, EmployeeDataService>(appURI);
...
```
L'utilizzo all'interno dei servizi rimane il medesimo, perché l'Helper continua a ritornare un'istanza HttpClient.\
Quindi il codice sotto funziona senza modifiche:
```csharp
private readonly HttpClient _htppClient;

public EmployeeDataService(HttpClient httpClient)
{
    _httpClient = httpClient;
}
```

