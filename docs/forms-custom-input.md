# Custom Input

Per facilitare lo sviluppo dei form, sfruttando la struttura a _componenti_ tipica delle applicazioni Blazor, possiamo costruire dei componenti custom.

## Evoluzione di Select

Uno dei componenti più sfrutatti è sicuramente `InputSelect`.

Il componente `InputSelect` genera l'HTML del tag `<input>` e le relatove `option`.

Vediamo un esempio:

```csharp
<InputSelect id="jobcategory" @bind-Value="@JobCategoryId" class="form-control">
    @foreach (var jobCategory in JobCategories)
    {
        <option value="@jobCategory.JobCategoryId">
            @jobCategory.JobCategoryName
        </option>
    }
</InputSelect>
```


## Costruzione del componente

Per costruire un componente custom è necessario partire da una base.

Dato che AspNet.Core è un progetto _open source_ possiamo partire dai sorgenti del componente base.

Collegandoci al sito https://github.com/aspnet/AspNetCore possiamo ricercare _InputSelect_ e scaricare il relativo sorgente (vedasi [qui](https://github.com/dotnet/aspnetcore/blob/15f341f8ee556fa0c2825cdddfe59a88b35a87e2/src/Components/Web/src/Forms/InputSelect.cs)).

### Evoluzione

Creare un componente derivato che evolva il componente originale è relativamente semplice.

Abbiamo due strade possibili:

1. estendere la classe base `InputSelect<TValue>`
2. creare un componente ex novo estendendo `InputBase<TValue>`

### Sorgente di partenza

Questo è il sorgente completo del componente `InputSelect`:

```csharp
// Copyright (c) .NET Foundation. All rights reserved.
// Licensed under the Apache License, Version 2.0. See License.txt in the project root for license information.

using System.Diagnostics.CodeAnalysis;
using Microsoft.AspNetCore.Components.Rendering;

namespace Microsoft.AspNetCore.Components.Forms
{
    /// <summary>
    /// A dropdown selection component.
    /// </summary>
    public class InputSelect<TValue> : InputBase<TValue>
    {
        /// <summary>
        /// Gets or sets the child content to be rendering inside the select element.
        /// </summary>
        [Parameter] public RenderFragment? ChildContent { get; set; }

        /// <inheritdoc />
        protected override void BuildRenderTree(RenderTreeBuilder builder)
        {
            builder.OpenElement(0, "select");
            builder.AddMultipleAttributes(1, AdditionalAttributes);
            builder.AddAttribute(2, "class", CssClass);
            builder.AddAttribute(3, "value", BindConverter.FormatValue(CurrentValueAsString));
            builder.AddAttribute(4, "onchange", EventCallback.Factory.CreateBinder<string?>(this, __value => CurrentValueAsString = __value, CurrentValueAsString));
            builder.AddContent(5, ChildContent);
            builder.CloseElement();
        }

        /// <inheritdoc />
        protected override bool TryParseValueFromString(string? value, [MaybeNullWhen(false)] out TValue result, [NotNullWhen(false)] out string? validationErrorMessage)
            => this.TryParseSelectableValueFromString(value, out result, out validationErrorMessage);
    }
}
```

Se volessivo ad esempio creare un componente per filtrare i valori sulla base di un campo di testo dovremo effettuare l'override del metodo `BuildRenderTree`.

A quel punto il nuovo componente potrebbe essere utilizzato direttamente dalle pagine Razor:

```csharp
<CustomInputSelect Filtering="true" ... />
```

