DanceDanceSnake é uma variação do clássico jogo Snake, desenvolvido em C# utilizando o motor gráfico MonoGame.
O objetivo do jogo é controlar uma cobra para apanhar frutas, evitando colisões com paredes ou com o próprio corpo.

## O porquê da Escolha
Escolhemos o projeto DanceDanceSnake por ser um jogo 2D simples e bem estruturado.
O projeto utiliza MonoGame, permitindo a aprendizagem de desenvolvimento de jogos 2D em C#.
A organização do código facilita a manutenção e futuras melhorias, com classes separadas para a cobra, o campo de jogo e a gestão de níveis.

## Estrutura Geral 

- DanceDanceSnake.sln – Ficheiro de solução do Visual Studio. 
- DanceDanceSnake.csproj – Projeto principal da aplicação. 
- Program.cs – Ponto de entrada da aplicação. 

## Pastas principais 

__Camera__ 

Camera.cs – Lógica da câmara usada para renderizar o jogo. 

__Content__

Contém recursos gráficos (sprites, fontes, ecrãs), o ficheiro .mgcb (MonoGame Content Builder), e versões compiladas em .xnb. 

__Data__

Globals.cs – Provavelmente contém constantes e variáveis globais usadas pelo jogo. 

__Input__ 

Interfaces e implementações para controlo via teclado ou gamepad. 
IInput.cs, KeyboardInput.cs, GamepadInput.cs, etc. 

__Level__ 

Definições dos níveis e terreno do jogo. 
Playground.cs, TestLevel.cs, etc. 

__Registration__ 

ApplicationRegistry.cs – Possivelmente trata da injeção de dependências ou registo de serviços. 

__Snake__

Componentes relacionados com a cobra (jogador). 
Snake.cs, SnakeBodyPart.cs. 

__StateMachine e StateMachine/States__ 

Implementação de uma máquina de estados para gerir os diferentes estados da aplicação: 
Menu inicial, jogo em curso, pausa, créditos, fim de jogo, etc. 

Ex.: ApplicationStateGameRunning.cs, ApplicationStateTitleScreen.cs, etc. 

__Initialization__ 

Inicializadores como resolução do ecrã, com interface e implementação. 

__Wrapper__ 

Agrupamento de classes "wrapper" que abstraem interações com o MonoGame: 
GameWrapper, GraphicsDeviceWrapper, SpriteBatchWrapper, ContentWrapper. 

__3rdParty__ 

Dependências externas organizadas por biblioteca: 
Lamar, Stateless, SharpDX, MonoGame, NUnit, entre outras. 

Forma 

__Extras__ 

ToDo.txt e Ideen.txt – Notas do desenvolvimento com ideias e tarefas pendentes. 
Binários compilados (.exe, .dll) e ficheiros de configuração nos diretórios bin/ e obj/. 

## Análise de Pastas

__1-Pasta Input__

Esta pasta trata de todas as formas de entrada do jogador, como teclado ou comandos (gamepads). A ideia é usar interfaces para abstrair os controlos, o que permite mudar o tipo de entrada sem alterar o resto do código do jogo. 

## 🧩 Ficheiros principais: 

__1. IInput.cs__ 

__Interface que define o contrato de entrada: o que qualquer sistema de input deve saber fazer.__ 

__Métodos típicos:__ 

IsUpPressed() 
IsDownPressed() 
IsLeftPressed() 
IsRightPressed()
IsActionPressed() 
Ou seja, tanto o teclado como o gamepad implementam esta interface — o jogo apenas interage com IInput, sem saber de onde vem a entrada. 

__2. KeyboardInput.cs__

Implementa IInput para usar o teclado como controlo. 
Usa Keyboard.GetState() do MonoGame para detetar se teclas como W, A, S, D ou setas estão pressionadas. 
Também pode verificar se a tecla de espaço ou enter foi carregada para acções especiais. 

__3. GamepadInput.cs__ 

Outra implementação de IInput, mas para comandos (gamepads). 
Usa a API GamePad.GetState() para ler o D-pad ou joysticks. 
Permite que dois jogadores usem comandos distintos. 

__4. InputFactory.cs__ 

Classe que escolhe dinamicamente qual a implementação de IInput usar (teclado ou comando). 
Facilita a troca entre modos de controlo (ex.: se houver comando ligado, usa o comando; senão, usa teclado). 

__5. MultiInput.cs__ 

Esta é interessante: combina várias fontes de entrada. 
Exemplo: permite que o jogador use simultaneamente teclado e comando. 
Ideal para dar mais liberdade ao jogador ou para multiplayer local. 

## 2-Pasta Level 
Esta pasta contém classes que representam o cenário de jogo, incluindo o mapa (chamado Playground), os objetos que nele existem, e os níveis de teste ou produção. 

## Ficheiros principais: 

__1. Playground.cs__ 

Classe central que representa o mapa ou “campo de jogo”. 
Contém: 
A grelha de células (tiles). 
Informações sobre obstáculos, espaços livres, paredes, etc. 
Métodos como: 
IsOccupied(position) – verifica se há algo nessa posição. 
SpawnFood() – coloca comida aleatória no mapa. 
Update() e Draw() – atualizam e desenham o nível. 
Pensa nisto como o “tabuleiro” onde tudo acontece. A cobra, os itens, e os limites estão todos referenciados aqui. 

__2. TestLevel.cs__ 

Implementa um nível de teste baseado em Playground. 
Provavelmente: 
Define o tamanho da grelha (ex.: 20x20). 
Coloca alguns obstáculos fixos para testar colisões. 
Inicializa comida ou outras entidades. 
Este nível serve para desenvolvimento, para ver se a lógica da cobra, colisões e limites está a funcionar bem. 

__3. PlaygroundTile.cs__ 

Representa uma única célula da grelha. 
Cada tile pode conter: 
Nada (espaço livre), 
Uma parede, 
Comida, 
Parte da cobra, etc. 
Pode ter propriedades como: 
IsBlocked, IsWalkable, TileType. 

__4. PlaygroundFactory.cs__ 

Classe responsável por criar instâncias de Playground. 
Abstrai a criação de níveis, útil para alternar entre vários layouts ou níveis dinamicamente. 

__5. TileType.cs__ 

Enumeração que define os tipos possíveis de tiles: 
Empty, Wall, Food, Snake, etc.
Isto permite tratar os diferentes tipos de elementos do mapa com lógica simples e clara. 

## 3-Pasta Snake

Esta pasta contém as classes responsáveis pela lógica do movimento da cobra, o seu crescimento, e o controlo das suas partes (cabeça e corpo). 
Ficheiros principais: 

__1. Snake.cs__ 

Esta é a classe principal da cobra. 
Responsável por: 
Criar a cobra (com cabeça e segmentos de corpo). 
Atualizar o movimento com base na direção atual. 
Adicionar novos segmentos quando a cobra “cresce”. 
Verificar colisões (com o próprio corpo, obstáculos, etc.). 
Mantém uma lista de segmentos do corpo e gere o seu posicionamento. 

__2. SnakeBodyPart.cs__ 

Representa um único segmento do corpo da cobra. 
Cada parte sabe: 
A sua posição anterior. 
Qual a próxima posição a ocupar (segue a parte da frente). 
Serve para dar aquele efeito de “rasto” característico. 
 
__3. ISnake.cs__ 

Interface para a cobra — define o comportamento essencial que qualquer cobra deve ter. 
Move(), Grow(), Draw(), etc. 
Isto ajuda se quiseres ter mais do que um tipo de cobra (por exemplo, multiplayer ou IA). 

__4. SnakeController.cs__ 

Controla a cobra com base na entrada do jogador. 
Lê o input (por exemplo, através de IInput) e atualiza a direção da cobra. 
Pode também impedir que o jogador vire 180º (por exemplo, ir diretamente da esquerda para a direita). 

__5. SnakeDrawer.cs__ 

Responsável pelo desenho da cobra no ecrã. 
Usa SpriteBatch para desenhar a cabeça e os segmentos com as sprites corretas. 
Pode aplicar rotação ou escolher sprites diferentes para curvas, por exemplo. 

__Resumo de como funciona tudo junto?__ 

O SnakeController recebe os inputs. 
A classe Snake usa esses inputs para atualizar o movimento. 
Cada SnakeBodyPart atualiza a sua posição. 
O SnakeDrawer desenha tudo. 

## 4- Pasta StateMachine 

Esta pasta implementa uma máquina de estados, ou seja, um sistema que sabe em que estado o jogo está atualmente e o que deve acontecer nesse estado. 

__Pasta principal StateMachine__ 

Contém: 

ApplicationStateMachine.cs – A classe que gere os estados e trata das transições. 
IApplicationState.cs – Interface base para todos os estados. 
IStateFactory.cs e StateFactory.cs – Usadas para criar instâncias dos vários estados de forma limpa (injeção de dependências ou modularidade). 

__Subpasta: States__ 

Contém as classes concretas para cada estado do jogo: 
Exemplos: 
ApplicationStateTitleScreen.cs – Ecrã inicial (menu principal). 
ApplicationStateGameRunning.cs – Estado durante o jogo ativo. 
ApplicationStatePaused.cs – Quando o jogador pausa o jogo. 
ApplicationStateGameOver.cs – Fim de jogo. 
ApplicationStateCredits.cs – Ecrã de créditos. 

__Cada um destes ficheiros:__ 

Implementa uma interface ou classe base comum. 
Define métodos como: 
Enter() – O que acontece ao entrar neste estado. 
Update() – O que deve ser atualizado enquanto está neste estado. 
Draw() – O que deve ser desenhado no ecrã. 
HandleInput() – Reage ao input do jogador. 

__Vantagens desta organização__  

Torna o código muito mais limpo e modular. 
Permite adicionar ou alterar estados facilmente. 
Evita grandes blocos de if ou switch espalhados pelo código. 
Cada estado tem as suas responsabilidades isoladas. 

## Possíveis Melhorias Futuras Apresentadas
- Adicionar múltiplos níveis com dificuldades diferentes.
- Integrar sons e música de fundo.
- Animações suaves na movimentação da cobra.
- Modos de jogo extra (ex.: contra-relógio).

## Grupo:
Francisco Macedo nº31471
Gonçalo Ribeiro nº31459
Rodrigo Costa nº31468
