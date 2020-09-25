# Routing

## Server Side

In un'applicazione Blazor Server nel _ConfigureServices_ sono impostati due endpoint:

```csharp
app.UseEndpoint(ep => {
    ep.MapBlazorHub();
    ep.MapFallbackToPage("/_Host");
});
```
Il primo endpoint (`Hub`) attiva le connessioni _SignalR_.\
Il secondo endpoint (`Fallback`) serve a reindirizzare verso la pagina `_Host` tutte le rotte che non sono risolte per le Razor pages e le pagine MVC.

In pratica le pagine Blazor Server possono convivere all'interno di un'eco sistema misto formato da MVC, Razor pages e Blazor pages.

### Esempio

Request: `localhost/employees`

Match per MVC Controller o Razor Pages ?

- Sì => __Action__ method o __Razor__ Page
- No => Fallback __Host.cshtml__
    - Esiste un component Blazor che risolve la rotta?
        * Sì => __Blazor Component__
        * No => Blazor __404__

## Client Side (WebAssembly)

Un'applicazione Client Side non dispone dei _Circuits_ SignalR, quindi non necessita del primo endpoint visto sopra.

Allo stesso modo un'applicazione Client Side non può sfruttare le tecnologie _server side_ come Razor pages o MVC, quindi anche il secondo endpoint non è necessario.

