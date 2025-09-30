# Fundamentos Jetpack Compose Listas Lazy
  

## Membros do grupo
- Diogo Makoto Mano, 98446
- Thiago Ratão Passerini, 551351
- Gabriel Valério Gouveia, 552041

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



---


