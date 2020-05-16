# Teclas de atalho (key binding)

Para associar teclas de atalho, é preciso primeiro definir um `RoutedCommand`. Pode-se fazer isso diretamente no XAML:

```xml
<Window.Resources>
    <RoutedCommand x:Key="AlgumComando" />
</Window.Resources>
```

Feito isso, pode-se associar o comando à combinação de teclas:

```xml
<Window.InputBindings>
    <KeyBinding Command="{StaticResource AlgumComando}" Key="T" Modifiers="Ctrl" />
</Window.InputBindings>
```

Por fim, é preciso relacionar o comando a um método através do evento `Executed`:

```xml
<Window.CommandBindings>
    <CommandBinding Command="{StaticResource AlgumComando}" Executed="AlgumMetodo" />
</Window.CommandBindings>
```

O método precisa ter a seguinte assinatura:  

```csharp
private void AlgumMetodo(object sender, EventArgs e)
```

Caso a janela tenha uma barra de menus, é possível adicionar a tecla de atalho a um objeto `MenuItem` através da tag `InputGestureText`:

```xml
    <MenuItem Header="AlgumItem" Click="AlgumMetodo" InputGestureText="Ctrl+T" />
```
