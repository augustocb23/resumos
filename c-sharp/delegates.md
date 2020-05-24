# Delegados

>Os delegados fornecem um mecanismo de *associação tardia* no .NET. Associação tardia significa que você cria um algoritmo em que o chamador também fornece pelo menos um método que implementa parte do algoritmo.
[Documentação](https://docs.microsoft.com/pt-br/dotnet/csharp/delegates-overview)

Em outras palavras, são um meio fortemente tipado de se declarar um "método abstrato", a ser implementado por quem chamará a classe.

Algo interessante sobre delegados é que, nas versões atuais do C#, o compilador gera uma classe que extende de [MulticastDelegate](https://docs.microsoft.com/pt-br/dotnet/api/system.multicastdelegate). Esta classe permite que vários métodos sejam atribuídos ao delegado. Ao invocar o delegado, todos os métodos anexados a ele serão executados.

## Declaração

```csharp
namespace SomeNamespace
{
    public delegate void SomeDelegate(string str, int i);
}
```

Delegados não são variáveis, mas *tipos*. Portanto, uma instância de um delegado será um método que corresponde a assinatura desse delegado.

```csharp
class SomeClass
{
    private readonly string _name;
    public SomeDelegate someDelegate;

    public SomeClass(string name)
    {
        _name = name;
    }

    public SomeMethod(int i)
    {
        ...
        someDelegate(_name, i);
        ...
    }
}
```

Nesse exemplo, a classe define um delegado e chama-o através de um método. O primeiro parâmetro não é passado para o método, mas é um atributo definido pelo construtor da classe.

### Tipos genéricos

Para evitar que seja necessário criar um novo tipo para cada delegado, o .Net contém algumas classes genéricas. São as classes [Action](https://docs.microsoft.com/pt-br/dotnet/api/system.action), [Func\<TResult>](https://docs.microsoft.com/pt-br/dotnet/api/system.func-1) e [Predicate\<T>](https://docs.microsoft.com/pt-br/dotnet/api/system.predicate-1). Essas classes representam, respectivamente, um método que não retorna nada, um método que retorna  `TResult`, e um predicado, ou seja, um método que retorna um booleano.

Todas as classes podem receber até 16 tipos para definir os parâmetros do método. No caso da classe `Func`, o tipo do retorno será o último parâmetro informado.

```csharp
public Func<string>() MethodA;
public Func<int, string>() MethodA;
public Func<int, bool, string>() MethodA;
```

## Delegados nulos

O delegado deve ter sido atribuído ou a chamada irá lançar uma `NullReferenceException`.

Para contornar isso de forma silenciosa (sem tratar a exceção), usa-se o operador condicional nulo (`?.`) junto com o método `Invoke()`.

```csharp
public SomeMethod(int i)
{
    ...
    someDelegate?.Invoke(_name, i);
    ...
}
```

## Atribuição

Para fazer a atribuição é usado o operador `+=`.

```csharp
void SomeDelegateInvoke(string str, int i)
{
    Console.WriteLine($"{str}: {i}");
}
...

void OtherMethod()
{
    var someClass = new SomeClass("someName");
    someClass.someDelegate += SomeDelegateInvoke;
}
```

Da mesma forma, pode-se remover o método do delegado usando o operado `-=`.

Também pode-se definir usando um delegado anônimo ou uma expressão lambda, porém dessa forma o método não pode ser removido.

```csharp
void OtherMethod()
{
    var someClass = new SomeClass("someName");
    someClass.someDelegate += delegate(string str, int i) { Console.WriteLine($"{str}: {i}"); };
}
```

```csharp
void OtherMethod()
{
    var someClass = new SomeClass("someName");
    someClass.someDelegate += (string str, int i) => Console.WriteLine($"{str}: {i}");
}
```
