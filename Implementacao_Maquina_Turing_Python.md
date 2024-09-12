
# Aula: Implementando uma Máquina de Turing Simples em Python

## Objetivos da Aula:
1. Entender o conceito de uma Máquina de Turing.
2. Implementar uma Máquina de Turing simples em Python.
3. Executar a máquina para resolver um problema básico, como inverter uma sequência de 0s e 1s.

## Pré-requisitos:
- Conhecimento básico de Python (funções, listas, loops).
- Noções básicas de teoria da computação (opcional).

## 1. Introdução à Máquina de Turing

Uma **Máquina de Turing** é composta pelos seguintes componentes:

1. **Fita infinita**: Uma fita de comprimento infinito, dividida em células, onde cada célula pode conter um símbolo. Esta fita é usada tanto para entrada quanto para saída.
2. **Cabeça de leitura/escrita**: Esta cabeça pode ler e escrever símbolos na fita e mover-se para a esquerda ou para a direita.
3. **Tabela de transição**: Um conjunto de regras que determina as ações da máquina com base no estado atual e no símbolo sendo lido.
4. **Estado atual**: A máquina possui um conjunto de estados e sempre está em um estado específico. Há um estado inicial, e um ou mais estados de aceitação.

A máquina funciona lendo o símbolo da célula atual, consultando a tabela de transição para decidir o que fazer (escrever, mover, ou mudar de estado) e repetindo esse processo.

## 2. Implementação de uma Máquina de Turing em Python

Vamos implementar uma Máquina de Turing simples que inverte uma sequência de 0s e 1s na fita. A fita será uma lista de caracteres, e a cabeça de leitura/escrita se moverá sobre essa lista.

### Passo 1: Definir a Estrutura da Máquina de Turing

Criaremos uma classe chamada `TuringMachine` para encapsular toda a lógica:

```python
class TuringMachine:
    def __init__(self, tape, transition_function, initial_state, accepting_states):
        self.tape = list(tape)  # Representa a fita como uma lista de caracteres
        self.head_position = 0  # Posição inicial da cabeça de leitura/escrita
        self.current_state = initial_state  # Estado inicial
        self.transition_function = transition_function  # Função de transição (dicionário)
        self.accepting_states = accepting_states  # Estados de aceitação

    def step(self):
        # Executa um único passo da máquina de Turing.
        # Lê o símbolo atual
        current_symbol = self.tape[self.head_position]
        
        # Encontra a ação na função de transição
        key = (self.current_state, current_symbol)
        if key in self.transition_function:
            new_state, new_symbol, direction = self.transition_function[key]
            
            # Atualiza o estado da máquina
            self.tape[self.head_position] = new_symbol
            self.current_state = new_state

            # Move a cabeça de leitura/escrita
            if direction == 'R':
                self.head_position += 1
            elif direction == 'L':
                self.head_position -= 1
        else:
            # Se a chave não está na função de transição, para a máquina
            raise Exception("Transição não definida para o estado atual e símbolo.")

    def run(self):
        # Executa a máquina até alcançar um estado de aceitação.
        while self.current_state not in self.accepting_states:
            print(f"Estado atual: {self.current_state}, Fita: {''.join(self.tape)}, Cabeça: {self.head_position}")
            self.step()
        print(f"Estado final: {self.current_state}, Fita: {''.join(self.tape)}, Cabeça: {self.head_position}")
        print("Máquina de Turing terminou.")
```

### Passo 2: Definir a Função de Transição

A função de transição define as regras para a Máquina de Turing. Vamos criar uma função de transição simples que inverte os 0s para 1s e vice-versa:

```python
transition_function = {
    ('q0', '0'): ('q0', '1', 'R'),  # No estado 'q0', ao ler '0', escreve '1', move para a direita, e permanece em 'q0'
    ('q0', '1'): ('q0', '0', 'R'),  # No estado 'q0', ao ler '1', escreve '0', move para a direita, e permanece em 'q0'
    ('q0', ' '): ('q_accept', ' ', 'N')  # No estado 'q0', ao ler um espaço (fim da fita), vai para o estado de aceitação
}
```

### Passo 3: Criar a Fita de Entrada e Inicializar a Máquina

Vamos criar a fita de entrada e instanciar a Máquina de Turing:

```python
# Fita de entrada com espaço extra para representar o final
input_tape = "01010101 "

# Estados de aceitação
accepting_states = {'q_accept'}

# Inicializa a Máquina de Turing
tm = TuringMachine(tape=input_tape, transition_function=transition_function, initial_state='q0', accepting_states=accepting_states)
```

### Passo 4: Executar a Máquina de Turing

Agora, podemos executar a Máquina de Turing chamando o método `run`:

```python
tm.run()
```

## 3. Explicação do Funcionamento

Quando o código acima é executado, a máquina de Turing realiza as seguintes etapas:

1. Começa no estado `q0` e lê o primeiro símbolo da fita.
2. Com base na função de transição, decide o que escrever, para onde mover a cabeça (esquerda, direita ou nenhum movimento), e para qual estado mudar.
3. Continua esse processo até alcançar um estado de aceitação (`q_accept`), onde a máquina para.
4. A saída mostrará a fita em cada etapa, mostrando como os 0s e 1s são invertidos.

## 4. Conclusão

Nesta aula, aprendemos:

- O que é uma Máquina de Turing e seus componentes principais.
- Como implementar uma Máquina de Turing básica em Python.
- Como usar a máquina para resolver um problema simples de inversão de uma sequência de 0s e 1s.

Este é um exemplo básico que pode ser expandido para resolver problemas mais complexos, como reconhecimento de padrões ou simulação de algoritmos mais avançados. A implementação de uma Máquina de Turing ajuda a entender os fundamentos da teoria da computação e serve como base para estudar algoritmos e inteligência artificial.
