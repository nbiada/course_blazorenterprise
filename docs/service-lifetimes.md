# Service Lifetimes

### Singleton
Viene creata una nuova istanza __all'avvio dell'applicazione__ ed ogni richiesta successiva, _per qualsiasi client_, riceverà la medesima istanza.

### Scoped
Viene creata una nuova istanza __per connessione__. Se nell'ambito della connessione due classi richiedono il medesimo servizio riceveranno la medesima istanza.

### Transient
Viene creata una nuova istanza __ogni volta che c'è una richiesta__. Se due classi richiedono il medesimo servizio, _anche all'interno della medesima connessione_, riceveranno una nuova istanza.


---
## Differenze tra Client e Server Side

Da Chris Saint - https://chrissainty.com/service-lifetimes-in-blazor

### Client Side (WebAssembly)
| Operation | Singleton | Scoped | Transient |
| --------- | --------- | ------ | --------- |
| Load home page | e353550e | bed089e5 | d748aa15 |
| Navigate to counter page | e353550e | bed089e5 | 48fc9930 |
| Reload the page | 9d1dd585 | 273eb2da |  5ebd1782 |
| Open app in new incognito tab | 369fc493 | 657f12a9 | 903983b4 |


### Server Side
| Operation | Singleton | Scoped | Transient |
| --------- | --------- | ------ | --------- |
| Load home page | e353550e | bed089e5 | d748aa15 |
| Navigate to counter page | e353550e | bed089e5 | 48fc9930 |
| Reload the page | e353550e | 273eb2da |  5ebd1782 |
| Open app in new incognito tab | e353550e | 657f12a9 | 903983b4 |
