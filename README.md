# Trabalho-pratico-TDV

# DanceDanceSnakeahjcuiuvhuhaohsnlc

**DanceDanceSnake** é uma variação do clássico jogo **Snake**, desenvolvido em **C#** utilizando o motor gráfico **MonoGame**.  
O objetivo do jogo é controlar uma cobra para apanhar frutas, evitando colisões com paredes ou com o próprio corpo.

##  Descrição

- Controla a cobra através das teclas de direção.
- Cada fruta que apanhe faz a cobra crescer.
- Bater numa parede ou em ti próprio termina o jogo.
- As frutas aparecem aleatoriamente em locais livres no campo.

##  Tecnologias Usadas

- **C#**
- **MonoGame**

##  Estrutura do Projeto

- **Game1.cs**  
  Classe principal do jogo. Gere o ciclo de vida (`Initialize`, `LoadContent`, `Update`, `Draw`).

- **Snake.cs**  
  Movimentos, crescimento e colisão da cobra.

- **Playground.cs**  
  Representa o campo de jogo. Gere as paredes, frutas e locais livres.

- **PlaygroundSection.cs**  
  Representa cada célula individual do campo (parede, fruta ou vazio).

- **TestLevel.cs**  
  Cria um nível de jogo inicial para testes.

- **LevelManager.cs**  
  Gerir Niveis.

## Possíveis Melhorias Futuras Apresentadas

- Adicionar múltiplos níveis com dificuldades diferentes.
- Integrar sons e música de fundo.
- Animações suaves na movimentação da cobra.
- Modos de jogo extra (ex.: contra-relógio).

## Decisões Tomadas
Escolhemos o projeto DanceDanceSnake por ser um jogo 2D simples e bem estruturado.
O projeto utiliza MonoGame, permitindo a aprendizagem de desenvolvimento de jogos 2D em C#.
A organização do código facilita a manutenção e futuras melhorias, com classes separadas para a cobra, o campo de jogo e a gestão de níveis.

## Grupo:
Francisco Macedo nº31471
Gonçalo Ribeiro nº31459
Rodrigo Costa nº31468
