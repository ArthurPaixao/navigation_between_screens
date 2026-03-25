# Navigation Between Screens

Este projeto demonstra como utilizar o Jetpack Navigation com o Jetpack Compose no Android.

## Navegação com Argumentos

O Navigation Compose permite a passagem de argumentos entre telas de forma semelhante a URLs web. Os argumentos podem ser **Obrigatórios** (Mandatory) ou **Opcionais** (Optional).

### 1. Argumentos Obrigatórios (Mandatory Arguments)

Argumentos obrigatórios são aqueles que fazem parte do caminho principal da rota e, se não forem passados, a navegação falha ou leva a uma rota não encontrada. No Compose, você define a rota com os argumentos entre chaves `{}` separados por barras `/`.

**Exemplo no código:**
Na nossa rota `perfil`, estamos passando dois parâmetros obrigatórios: `nome` e `idade`.

**Declaração da Rota:**
```kotlin
composable(
    route = "perfil/{nome}/{idade}",
    arguments = listOf(
        navArgument("nome") { type = NavType.StringType },
        navArgument("idade") { type = NavType.IntType }
    )
) { navBackStackEntry ->
    val nome = navBackStackEntry.arguments?.getString("nome", "Usuário Genérico")
    val idade = navBackStackEntry.arguments?.getInt("idade", 0)
    
    PerfilScreen(
        navController = navController,
        nome = nome!!,
        idade = idade!!
    )
}
```

**Como Navegar:**
```kotlin
navController.navigate("perfil/Fulano de Tal/27")
```

Neste caso, a tela de Perfil *precisa* receber os dois dados no exato momento da navegação.

### 2. Argumentos Opcionais (Optional Arguments)

Argumentos opcionais são adicionados como *query parameters* (como em URLs de sites: `?param=valor`). Para que um argumento seja opcional, ele deve:
1. Ser passado via query (`?param={param}`).
2. Ter um `defaultValue` configurado ou aceitar `null` (nullable).

**Exemplo no código:**
Na nossa rota `pedidos`, estamos passando um argumento opcional chamado `cliente`.

**Declaração da Rota:**
```kotlin
composable(
    route = "pedidos?cliente={cliente}",
    arguments = listOf(navArgument("cliente") {
        defaultValue = "Cliente Genérico" // Define um valor padrão caso não seja passado
    })
) { navBackStackEntry ->
    val cliente = navBackStackEntry.arguments?.getString("cliente")
    
    PedidosScreen(
        navController = navController,
        cliente = cliente
    )
}
```

**Como Navegar:**
```kotlin
// Passando o parâmetro
navController.navigate("pedidos?cliente=Cliente XPTO")

// Ou sem passar parâmetro (irá usar o defaultValue "Cliente Genérico")
navController.navigate("pedidos")
```

### Resumo das Telas Modificadas
* `MenuScreen`: Atualizamos os botões para executar a navegação com as rotas criadas.
* `PerfilScreen`: Recebe os dados obrigatórios de `nome` e `idade` e os mostra na UI.
* `PedidosScreen`: Recebe o dado opcional `cliente` e mostra na UI.
* `MainActivity`: Centraliza o `NavHost` configurando a forma com que os argumentos são capturados da rota para serem entregues como instâncias do Kotlin para as funções Composable.
