# Fundamentos Jetpack Compose Listas Lazy

Este projeto é um aplicativo Android desenvolvido em Kotlin, utilizando Jetpack Compose para a construção de interfaces modernas e reativas. O objetivo principal é demonstrar o uso de listas Lazy (LazyColumn e LazyRow) para exibir e filtrar jogos favoritos por estúdio.

## Funcionalidades

- Exibição de uma lista de jogos favoritos.
- Filtro de jogos por nome do estúdio, via campo de texto ou seleção direta.
- Lista horizontal de estúdios (StudioCard) para filtro rápido.
- Botão de limpar filtro, exibido apenas quando um filtro está ativo.
- Interface moderna utilizando Material 3.

## Estrutura do Projeto

```
app/
 ├── src/
 │   ├── main/
 │   │   ├── java/
 │   │   │   └── carreiras/com/github/fundamentos_jetpack_compose_listas_lazy/
 │   │   │        ├── MainActivity.kt         # Tela principal e lógica de UI
 │   │   │        ├── components/
 │   │   │        │    ├── GameCard.kt       # Componente visual para jogos
 │   │   │        │    └── StudioCard.kt     # Componente visual para estúdios
 │   │   │        ├── model/
 │   │   │        │    └── Game.kt           # Modelo de dados para jogos
 │   │   │        ├── repository/
 │   │   │        │    ├── GameRepository.kt # Funções de acesso e filtro de dados
 │   │   │        │    └── ...
 │   │   │        └── ui/theme/              # Temas e estilos
 │   │   └── res/                            # Recursos (layouts, strings, etc)
 │   └── ...
 └── ...
```

## Como funciona

- A tela principal exibe uma lista de jogos e uma lista horizontal de estúdios.
- O usuário pode filtrar os jogos digitando o nome do estúdio ou clicando em um StudioCard.
- O filtro pode ser limpo facilmente com o botão "Limpar filtro".

## Como rodar o projeto

1. Clone este repositório:
   ```sh
   git clone <url-do-repositorio>
   ```
2. Abra o projeto no Android Studio.
3. Execute em um emulador ou dispositivo físico Android.

## Tecnologias utilizadas
- **Kotlin**
- **Jetpack Compose**
- **Material 3**
- **Gradle Kotlin DSL**

## Screenshots



## Membros do grupo
- Diogo Makoto Mano
- Thiago Ratão Passerini

---


