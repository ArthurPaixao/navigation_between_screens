# Navigation Between Screens - Checkpoint 1

## Descrição do Projeto
Este projeto é um aplicativo Android desenvolvido com Jetpack Compose que demonstra os fundamentos da navegação entre telas utilizando o componente **Jetpack Navigation Compose**. O aplicativo possui múltiplas telas (Login, Menu, Pedidos e Perfil) e implementa fluxos de navegação que envolvem passagem de dados (argumentos) entre essas telas de forma segura e tipada.

## Evolução da Navegação (Explicação dos Últimos Commits)

Abaixo está o detalhamento de como a passagem de dados foi implementada passo a passo no projeto:

### 1º Passo: Passagem de um Argumento Obrigatório (Nome)
Nesta etapa, começamos a enviar dados de uma tela para outra. A tela de Perfil passou a exigir um "nome" para ser exibida.

*   **`MainActivity.kt`:** A rota foi alterada para `composable(route = "perfil/{nome}")`. O `{nome}` na URL significa que a rota agora exige um parâmetro obrigatório. O valor foi extraído e passado para a função `PerfilScreen`.
*   **`MenuScreen.kt`:** A ação do botão passou a navegar enviando um dado: `navController.navigate("perfil/Fulano de Tal")`.
*   **`PerfilScreen.kt`:** A função Composable ganhou um novo parâmetro no construtor (`nome: String`), e o texto na tela tornou-se dinâmico: `"PERFIL - $nome"`.

### 2º Passo: Passagem de um Argumento Opcional (Cliente)
Aqui foi implementada a lógica de argumentos *opcionais* para a tela de Pedidos, usando a sintaxe de *query string* (`?chave=valor`).

*   **`MainActivity.kt`:** A rota foi substituída por `"pedidos?cliente={cliente}"`. Foi adicionada uma lista de argumentos definindo um valor padrão (`defaultValue = "Cliente Genérico"`), garantindo que o app não quebre caso o parâmetro não seja fornecido.
*   **`PedidosScreen.kt`:** A função ganhou o parâmetro `cliente: String?` (podendo ser nulo) e o texto foi atualizado para exibir essa informação.

### 3º Passo: Utilizando o Argumento Opcional na Navegação
Ajuste na chamada da navegação para testar a funcionalidade de parâmetros opcionais.

*   **`MenuScreen.kt`:** O botão que leva para a tela de pedidos foi atualizado para chamar a rota completa com o parâmetro: `navController.navigate("pedidos?cliente=Cliente XPTO")`.
*   Ao invés de exibir o "Cliente Genérico", a tela passou a capturar e exibir a String enviada na navegação.

### 4º Passo: Múltiplos Argumentos Tipados (Nome e Idade)
Evolução da tela de Perfil para receber dois parâmetros obrigatórios com validação de tipos de dados (String e Inteiro).

*   **`MainActivity.kt`:** A rota mudou para `"perfil/{nome}/{idade}"`. Foi introduzida a validação de tipos usando `navArgument` (`NavType.StringType` e `NavType.IntType`).
*   **`MenuScreen.kt`:** A chamada de navegação foi atualizada para `"perfil/Fulano de Tal/27"`.
*   **`PerfilScreen.kt`:** A função foi expandida para receber `idade: Int` além do `nome`. O layout foi atualizado para exibir ambas as informações formatadas na UI.
