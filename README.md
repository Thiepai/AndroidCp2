# Fundamentos Jetpack Compose Listas Lazy

# 🎮 Meus Jogos Favoritos  

Aplicativo Android desenvolvido em **Kotlin** utilizando **Jetpack Compose**.  
O objetivo é demonstrar os **fundamentos de listas Lazy (LazyColumn e LazyRow)**, aplicando filtros e compondo interfaces modernas com **Material 3**.  

---

## 📱 Demonstração  
Exemplo da tela principal com busca e filtros:  

<img width="450" height="902" alt="image" src="https://github.com/user-attachments/assets/2419176f-04e7-4314-bf17-a0451a468503" />
<img width="451" height="915" alt="image" src="https://github.com/user-attachments/assets/c8a3516e-163e-4a42-b011-b990f59fec23" />
<img width="445" height="897" alt="image" src="https://github.com/user-attachments/assets/3db4aacd-df3f-4114-92ea-0964ccd31fab" />

---

## Mudanças no código
Item 1 — “Ao filtrar usar a caixa de texto, mostrar um botão (text) para ‘Limpar o filtro’”
Arquivo: MainActivity.kt → dentro de GamesScreen
1) Criar uma flag simples para saber se há filtro ativo
 @Composable
 fun GamesScreen(modifier: Modifier = Modifier) {
     var searchTextState by remember { mutableStateOf("") }
     var gamesListState by remember { mutableStateOf(getAllGames()) }
+    val hasActiveFilter = searchTextState.isNotBlank()


Explicação:
hasActiveFilter fica true sempre que o usuário digitou algo. Usamos essa flag para exibir o botão “Limpar filtro” somente quando há filtro ativo.

2) Tornar o botão “Limpar filtro” um TextButton Material3 e exibir apenas quando hasActiveFilter for true
- // Botão de limpar filtro
- if (searchTextState.isNotEmpty() || gamesListState != getAllGames()) {
-     Text(
-         text = "Limpar filtro",
-         modifier = Modifier
-             .padding(top = 8.dp)
-             .fillMaxWidth()
-             .clickable {
-                 searchTextState = ""
-                 gamesListState = getAllGames()
-             },
-         fontWeight = FontWeight.SemiBold,
-         color = androidx.compose.ui.graphics.Color.Blue
-     )
- }
+ // Botão de limpar filtro (aparece só se houver texto no filtro)
+ if (hasActiveFilter) {
+     androidx.compose.material3.TextButton(
+         onClick = {
+             searchTextState = ""
+             gamesListState = getAllGames()
+         },
+         modifier = Modifier.padding(top = 8.dp)
+     ) {
+         Text("Limpar filtro")
+     }
+ }


Explicação:

O enunciado pede “botão (text)”. Usamos TextButton do Material3 (sem mudar a lógica).

A visibilidade do botão depende apenas do texto do filtro (claro e previsível).

3) A busca via lupa usa a mesma base de estado
 OutlinedTextField(
     value = searchTextState,
     onValueChange = { searchTextState = it },
     modifier = Modifier.fillMaxWidth(),
     label = { Text(text = "Nome do estúdio") },
     trailingIcon = {
-        IconButton(onClick = { gamesListState = getGamesByStudio(searchTextState) }) {
+        IconButton(onClick = { applyFilter(searchTextState) }) { // ver função no Item 3
             Icon(
                 imageVector = Icons.Default.Search,
                 contentDescription = ""
             )
         }
     }
 )


Explicação:
No clique da lupa, aplicamos o filtro (e, como há texto, o botão “Limpar filtro” aparece). A chamada applyFilter(...) será criada no Item 3 para padronizar a lógica.

Item 2 — “Ao clicar no botão ‘Limpar filtro’, deve limpar o filtro executado”

(continuação do bloco acima)

TextButton limpa tudo no onClick
TextButton(
  onClick = {
    searchTextState = ""        // limpa o texto da busca
    gamesListState = getAllGames() // restaura a lista original
  }
) { Text("Limpar filtro") }


Explicação:

Zera o texto → hasActiveFilter fica false → o botão some automaticamente.

Restaura a lista original com getAllGames().

Item 3 — “Ao clicar em um estúdio na LazyRow, deve efetuar o mesmo filtro; deve mostrar também o botão para limpar filtro”
Arquivo: StudioCard.kt
1) Trocar o parâmetro de Game para studio: String (card mostra só o nome do estúdio)
- fun StudioCard(game: Game, onClick: (() -> Unit)? = null) {
+ fun StudioCard(studio: String, onClick: (() -> Unit)? = null) {
     Card(modifier = Modifier
         .size(100.dp)
         .padding(end = 4.dp)
         .clickable(enabled = onClick != null) { onClick?.invoke() }) {
         Column(
             verticalArrangement = Arrangement.Center,
             horizontalAlignment = Alignment.CenterHorizontally,
             modifier = Modifier.fillMaxSize()
         ) {
-            Text(text = game.studio)
+            Text(text = studio)
         }
     }
 }

Preview do StudioCard
- StudioCard(game = Game(1, "Example Game", "Example Studio", 2023))
+ StudioCard(studio = "Example Studio")


Explicação:
A LazyRow deve exibir estúdios (não jogos). O card recebe só a String do estúdio e fica clicável via onClick.

Arquivo: MainActivity.kt → dentro de GamesScreen
2) Criar uma função única de filtro e usá-la tanto na lupa quanto no clique do estúdio
 var gamesListState by remember { mutableStateOf(getAllGames()) }
 val hasActiveFilter = searchTextState.isNotBlank()
 
+ // Função única para aplicar o MESMO filtro em qualquer origem (texto ou clique no estúdio)
+ fun applyFilter(input: String) {
+     searchTextState = input
+     gamesListState = getGamesByStudio(input)
+ }


Explicação:
Centraliza a lógica: reduz duplicação e garante que o filtro seja idêntico nas duas entradas (exigência do item 3).

3) Gerar estúdios únicos a partir da lista atual e usar StudioCard(studio = …)
- LazyRow(){
-     items(gamesListState){ game ->
-         StudioCard(game = game, onClick = {
-             searchTextState = game.studio
-             gamesListState = getGamesByStudio(game.studio)
-         })
-     }
- }
+ // estúdios únicos para a faixa horizontal
+ val studios = remember(gamesListState) {
+     gamesListState.map { it.studio }.distinct()
+ }
+ LazyRow() {
+     items(studios) { studio ->
+         StudioCard(studio = studio, onClick = {
+             applyFilter(studio) // mesmo filtro do texto
+         })
+     }
+ }


Explicação:

A LazyRow agora mostra cada estúdio uma única vez (melhor UX).

Ao clicar num estúdio, chamamos applyFilter(studio) — idêntico ao filtro por texto → o botão “Limpar filtro” aparece porque searchTextState recebe o nome do estúdio.

4) Ajustar Previews no fim do arquivo (se existirem)
- StudioCard(game = Game(1, "Example Game", "Example Studio", 2023))
+ StudioCard(studio = "Example Studio")


---

## ✨ Funcionalidades  
- Exibição de uma lista de jogos favoritos (**LazyColumn**).  
- Filtro de jogos por **nome do estúdio**:  
  - Digitação no campo de texto.  
  - Seleção rápida através da lista horizontal (**LazyRow**).  
- Botão **"Limpar filtro"** exibido apenas quando um filtro está ativo.  
- Interface moderna seguindo padrões do **Material 3**.  

---

## 🗂️ Estrutura do Projeto  

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


---

## ⚙️ Como funciona  
1. A tela inicial exibe todos os jogos cadastrados.  
2. O usuário pode filtrar os jogos:  
   - Digitando o nome do estúdio no campo de busca.  
   - Tocando em um cartão de estúdio (**StudioCard**).  
3. O filtro pode ser removido facilmente com o botão **"Limpar filtro"**.  

---

## Tecnologias utilizadas
- **Kotlin**
- **Jetpack Compose**
- **Material 3**
- **Gradle Kotlin DSL**



## Membros do grupo
- Diogo Makoto Mano, 98446
- Thiago Ratão Passerini, 551351
- Gabriel Valério Gouveia, 552041

---


