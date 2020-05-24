# Eventos

>Os eventos são uma forma de um objeto difundir (para todos os componentes interessados do sistema) que algo aconteceu. Qualquer outro componente pode assinar ao evento e ser notificado quando um evento for gerado.
[Documentação](https://docs.microsoft.com/pt-br/dotnet/csharp/events-overview)

São uma implementação simplificada do padrão de projeto Observer ([Wikipedia](https://pt.wikipedia.org/wiki/Observer), [Microsoft Docs](https://docs.microsoft.com/pt-br/dotnet/standard/events/observer-design-pattern)). Para uma implementação mais completa, que permita um controle maior dos inscritos, usa-se as interfaces `IObservable<T>` e `IObserver<T>`.

## Declaração

Eventos são declarados usando a palavra reservada `event` e possuem o tipo [EventHandler](https://docs.microsoft.com/pt-br/dotnet/api/system.eventhandler). Essa classe pode receber um parâmetro indicando o tipo de retorno do objeto, permitindo que o evento envie um dado quando emitido.

```csharp
public event EventHandler Disconnected;
public event EventHandler<int> ConnectionIdChanged;
```

## Inscrição

Eventos são [delegados](delegates.md) que recebem como parâmetro o objeto que disparou (`sender`) e o argumento (`eventArgs`), se houver. Portanto, para se inscrever no evento, o método deve possuir essa assinatura.

```csharp
void OnConnectionIdChanged(object sender, int id)
{
    Console.WriteLine($"New connection id: { id }.");
}
...
void ConfigureEvents()
{
    _connection.Disconnected += delegate { Console.WriteLine("Connection lost."); };
    _connection.ConnectionIdChange += OnConnectionIdChanged;
}
```

## Disparo do evento

Para disparar o evento, basta chamar o método `Invoke`, passando `this` e o dado a ser retornado, ou `EventArgs.Empty`.

É importante sempre usar o operador condicional `?.` para evitar que o evento seja disparado sem que haja nenhum assinante, ou isto gerará uma `NullReferenceException`.

```csharp
private OnDisconnect()
{
    Disconnected?.Invoke(this, EventArgs.Empty);
}
```

## Convenções

Alguns padrões usados pelo .Net em eventos:

- Nomes de evento são sempre escritos no passado (Connected, Disconnected, Changed, etc)
- O método que dispara o evento tem o prefixo `On` seguido do nome do evento (OnConnected)
- Eventos devem ser *opcionais*: o sistema deve funcionar corretamente mesmo que não hajam assinantes
