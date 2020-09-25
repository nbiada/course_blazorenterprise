# Blazor Helpers
Per facilitare lo sfruttamento dei servizi di recupero dati via Rest API Blazor mette a disposizione una serie di Helper.

Un servizio _classico_ recupera i dati così:

```csharp
public async Task<IEnumerable<Employee>> GetAllEmployees()
{
    return await JsonSerializer.DeserializeAsync<IEnumerable<Employee>> (await _httpClient.GetStreamAsync($"api/employee"), new JsonSerializer...);
}
```

Per sfruttare gli Helper dobbiamo caricare il pacchetto Nuget dedicato `Microsoft.AspNetCore.Blazor.HttpClient`.

```csharp
public async Task<IEnumerable<Employee>> GetAllEmployees() 
{
    return await _client.GetJsonAsync<Employee[]>($"api/employee");
}
```
allo stesso modo per recuperare un singolo record:
```csharp
[...]
    return await _client.GetJsonAsync<Employee>($"api/employee/{employeeId}");
```
Per l'aggiornamento dei dati è simile:
```csharp
public async Task AddJob(Jon newJob)
{
    await _client.PostJsonAsync("jobs", newJob);
}
```
oppure `PutJsonAsync` e via dicendo.
