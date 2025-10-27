# 🚥 Projeto Semáforo com LEDs e Arduino

## 1. Visão Geral

O objetivo deste projeto é simular o funcionamento básico de um semáforo de trânsito em escala reduzida, utilizando Diodos Emissores de Luz (LEDs) nas cores padrão (Vermelho, Amarelo e Verde) e uma placa microcontroladora Arduino.

O projeto é dividido em duas etapas principais: a montagem do circuito eletrônico (hardware) e a implementação da lógica de temporização (software).

## 2. Componentes e Montagem (Hardware)

### 2.1. Lista de Materiais

| Componente | Quantidade | Função |
| :--- | :---: | :--- |
| Placa Microcontroladora (Arduino UNO ou similar) | 1 | Controla a lógica e temporização. |
| LED Vermelho | 1 | Sinalização de Parada. |
| LED Amarelo | 1 | Sinalização de Atenção / Transição. |
| LED Verde | 1 | Sinalização de Livre. |
| Resistor de $1k\Omega$ | 3 | Limita a corrente e protege cada LED. |
| Protoboard | 1 | Base para montagem do circuito sem solda. |
| Fios Jumper | 4 | Conexões elétricas. |

### 2.2. Detalhes da Resistência (Cálculo e Seleção)

A escolha do resistor é crucial para limitar a corrente e proteger os LEDs, que serão conectados à saída de $5V$ dos pinos digitais do Arduino.

**Dados Considerados:**
* Tensão da Fonte ($V_{fonte}$): $5V$
* Corrente de Operação Desejada ($I$): **$5mA$ ou $0.005A$**
* Tensão Direta Assumida do LED ($V_{LED}$):
    * LEDs Vermelho e Amarelo: $V_{LED} = 2.0V$
    * LED Azul (Cálculo Teórico): $V_{LED} = 3.0V$

**Fórmulas Utilizadas:**

1.  **Tensão no Resistor ($V_{resistor}$):**
    $$V_{resistor} = V_{fonte} - V_{LED}$$
2.  **Resistência Mínima Necessária ($R$ - Lei de Ohm):**
    $$R = \frac{V_{resistor}}{I}$$

**Cálculos de Resistência Mínima ($R_{mín}$):**

| LED | $V_{LED}$ | $V_{resistor}$ ($5V - V_{LED}$) | $R_{mín}$ ($V_{resistor} / 0.005A$) |
| :--- | :---: | :---: | :---: |
| Vermelho/Amarelo | $2.0V$ | $5V - 2V = 3V$ | $R_{mín} = \frac{3V}{0.005A} = 600\Omega$ |
| Azul (Teórico) | $3.0V$ | $5V - 3V = 2V$ | $R_{mín} = \frac{2V}{0.005A} = 400\Omega$ |

**Resistor Selecionado:**
Foi escolhido um resistor comercial de **$1k\Omega$ ($1000\Omega$)** para todos os LEDs. Este valor é superior a ambos os mínimos calculados ($600\Omega$ e $400\Omega$). Utilizar uma resistência maior resulta em uma corrente elétrica menor que $5mA$, o que garante maior segurança, longevidade e proteção dos LEDs.

### 2.3. Esquema de Conexão (Wiring)

Cada LED (ânodo/pino longo) é conectado a um pino digital do Arduino através de um resistor. O cátodo (pino curto) de todos os LEDs é conectado ao pino de terra (`GND`) da Protoboard/Arduino.

| LED | Pino Digital do Arduino | Componente de Proteção |
| :--- | :---: | :--- |
| **Vermelho** | **13** | Resistor $1k\Omega$ |
| **Amarelo** | **12** | Resistor $1k\Omega$ |
| **Verde** | **11** | Resistor $1k\Omega$ |

## 3. Lógica e Programação (Software)

### 3.1. Lógica do Ciclo

O código implementa o ciclo de um semáforo convencional:

1.  **Vermelho** (Parar): **6 segundos**
2.  **Verde** (Livre): **4 segundos**
3.  **Amarelo** (Atenção/Transição): **2 segundos**
4.  O ciclo se repete: Amarelo $\rightarrow$ Vermelho.

### 3.2. Código Arduino (Sketch)

```cpp
// Função de configuração inicial, executada uma única vez ao iniciar o Arduino
void setup() {
  pinMode(13, OUTPUT); // Define o pino 13 como saída (geralmente usado para LED vermelho)
  pinMode(12, OUTPUT); // Define o pino 12 como saída (pode ser LED amarelo)
  pinMode(11, OUTPUT); // Define o pino 11 como saída (pode ser LED verde)
}

// Função principal que roda continuamente em loop
void loop() {
  digitalWrite(13, HIGH); // Liga o LED conectado ao pino 13
  delay(6000);            // Aguarda 6 segundos
  digitalWrite(13, LOW);  // Desliga o LED do pino 13

  digitalWrite(12, HIGH); // Liga o LED conectado ao pino 12
  delay(2000);            // Aguarda 2 segundos
  digitalWrite(12, LOW);  // Desliga o LED do pino 12

  digitalWrite(11, HIGH); // Liga o LED conectado ao pino 11
  delay(2000);            // Aguarda 2 segundos
  digitalWrite(11, LOW);  // Desliga o LED do pino 11
}
```

## 4. Mídia do Projeto

### 4.1. Circuito Físico

Abaixo estão as imagens da montagem final do semáforo na protoboard, conectado ao Arduino.

<div>
  <img src="./imagens/circuito_fisico.jpg" alt="Imagem do circuito fisico" style="height:50%, width:50%"/>
</div>



### 4.2. Demonstração em Vídeo

O vídeo a seguir demonstra o funcionamento do ciclo do semáforo, seguindo a lógica de temporização programada (6s Vermelho, 4s Verde, 2s Amarelo).

[Link para o Vídeo de Demonstração](`https://drive.google.com/drive/folders/1EM8jXSWpORsn3lkqzj7SS2wOz4eqlzig?usp=drive_link`)