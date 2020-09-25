# Organizzazione dell'applicazione

 ## La Solution
 La Solution è divisa in __5 progetti__.

 1. __UI__
    
    a. Components\
    _componenti ad uso dell'interfaccia (modali)_

    b. Pages\
    _le pagine con i relativi Code Behind_

    c. Interfaces\
    _interfaccie ai servizi_

    d. Services\
    _servizi per recupero dati via API_

    e.Shared\
    _NavMenu e MainLayout_

2. __Shared__

    modelli dati (comprese data annotations per validazione)

3. __Data__

    DbContext e Repositories (con relative interfaccie) per il recupero e aggiornamento dei dati

4. __Api__

    server App. Contiene i controllers, lo Startup e l'appsettings

5. __ComponentsLibrary__

    contiene l'integrazione con OpenStreetmap, pensata per diventare un package NuGet

### Analisi
Nell'organizzazione che utilizzo __Api__ e __Data__ sono all'interno del medesimo progetto.\
Mantenerli separati ha senso solamente se in prospettiva si pensa di utilizzare un'altra forma di fruizione dei dati oltre alla Rest API.\
Potrebbe essere un ragionamento interessante in funzione di avere anche GraphQL. Da valutare.\
_Dipendenze: Data -> Shared, Api -> Data, Shared_

La __ComponentsLibrary__ potrebbe essere un'idea interessante, ma solamente per aree fortemente trasversali.\
L'esempio della mappa via OpenStreet è una buona idea.\
_Dipendeze: nessuna_


Il progetto __Shared__ accentra modelli dati e validazioni.\
Normalmente mantengo separate le validazioni per ottimizzare i View Model.\
Qui è meglio mantenere _solamente_ i modelli dati, senza validazioni.\
_Dipendenze: nessuna_

La __UI__ è correttamente separata.\
_Dipendenze: Shared, ComponentsLibrary_

