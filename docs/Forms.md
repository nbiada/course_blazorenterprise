# Validazione

Per aggiungere validazioni complesse installare il package `Microsoft.AspNetCore.Components.DataAnnotations.Validation`.

Per attivare il nuovo validatore è necessario sostituire il classico `DataAnnotationsValidator`, presente come tag subito sotto la `EditForm`, con il `ObjectGraphDataAnnotationsValidator`.

Per validare strutture complesse:

```csharp
public class Employee
{
    public int Id {get; set;}
    [Required]
    public string Name {get; set;}
    [ValidateComplexType]
    public Address Address {get; set;}
}
```
In questo modo se il tipo `Address` contiene dei campi con dei __Required__ verranno correttamente valutati nella form quando verranno inseriti come:
```csharp
<ValidationMessage for="(() => context.Address.City)" />
```
senza il `ValidateComplexType` il sistema li ignorerebbe senza alcun messaggio o validazione.

## Validatori Custom
Per aggiungere una validazione custom per la maggiore età:
```csharp
public class BirthdayValidator: ValidationAttribute
{
    public int MinimumAge {get; set;}

    protected override ValidationResult IsValid(object value, ValidationContext validationContext)
    {
        DateTime birthDate;
        if (DateTime.TryParse(value.ToString(), out birthDate))
        {
            if (birthDate < DateTime.Now.AddYears(MinimumAge * -1))
            {
                return null;
            }
            else
            {
                return new ValidationResult($"Employee must be at least {MinimumAge}.", new[] { validationContext.MemberName });
            }
        }

        return new ValidationResult("Invalid birth date.", new [] { validationContext.MemberName });
    }
}
```
utilizzo:
```csharp
...
[BirthdayValidator(MinimumAge = 18)]
public DateTime BirthDate {get; set;}
```

## Model Level Validation
Esempio per Longitudine e Latitudine.

Per prima cosa è necessario derivare la classe dall'interfaccia `IValidatableObject`, quindi:

```csharp
public class Address: IValidatableObject
{

}
```
poi è necessario implementare l'interfaccia attraverso il metodo previsto:
```csharp
public IEnumerable<ValidationResult> Validate(ValidationContext validationContext)
{
    var errors = new List<ValidationResult>();

    if (Latitude != 0 && Longitude == 0) 
    {
        errors.Add(new ValidationResult( "Longitude is required if Latitude has a value.",
        new [] {nameof(Longitude)}));
    }
    else if 
    ...
}
```