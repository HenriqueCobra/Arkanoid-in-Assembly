# Arkanoid em Assembly (Processador P3)

Um clone do clássico Arkanoid desenvolvido puramente em linguagem de montagem (Assembly 16-bit) para a arquitetura do Processador P3. Este projeto não utiliza engines ou bibliotecas externas, gerenciando diretamente o mapeamento de memória de vídeo, interrupções de hardware e a pilha de execução.

![Demo do Arkanoid](https://github.com/user-attachments/assets/95a75f57-239e-4d62-944f-320e869aaf98)

## 🧠 Arquitetura e Decisões Técnicas

O jogo foi construído com foco no controle de baixo nível e otimização de ciclos de instrução:

* **I/O Mapeado em Memória:** A renderização da tela é feita escrevendo diretamente nos endereços de periféricos do P3 (`FFFCh` para o cursor e `FFFEh` para o caractere), manipulando a matriz do mapa caractere por caractere.
* **Game Loop Orientado a Interrupções:** Em vez de um loop síncrono bloqueante (polling), o jogo é assíncrono.
  * `INT15` (Timer): Controla a física da bola e atualização da tela.
  * `INT1` e `INT2` (Teclado): Movimentação da nave (paddle).
  * `INT3`: Reinício do estado da máquina.
* **Física e Colisão em Matriz:** O cálculo de colisão da bola com os blocos e a nave é feito através de offsets matemáticos diretos na memória sequencial, verificando os limites da tela (`TOP_BOUND`, `RIGHT_BOUND`, etc.).
* **Gerenciamento de Estado (Stack):** Uso rigoroso de `PUSH` e `POP` em todas as rotinas e sub-rotinas (como `BlockColision` e `MoveBall`) para preservar o estado dos registradores `R1` a `R6` e evitar corrupção de dados durante as interrupções.

## 🚀 Como Executar

1. Compile o arquivo fonte (`.as`) utilizando o assembler do processador P3 para gerar o executável (`.exe`).
2. Inicie o simulador **p3sim** e carregue o arquivo `.exe` recém-compilado na memória.
3. Abra a interface do **Terminal de Texto** (onde a matriz do jogo será renderizada).
4. Configure os botões de interrupção (IVADs) na interface do simulador com os seguintes mapeamentos:
   * **INT 1:** Tecla da Direita (Move a nave para a direita)
   * **INT 2:** Tecla da Esquerda (Move a nave para a esquerda)
   * **INT 3:** Tecla/Botão de Restart (Reinicia o estado do jogo)
5. Inicie a execução do simulador (Run).
