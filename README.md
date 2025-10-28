# Semaforo offline

<img src="assets/image.jpg">

## Relatorio

Projeto de semáforo offline para Arduino Uno que controla três LEDs (verde, amarelo e vermelho) usando millis() para temporizações não bloqueantes; o sistema realiza transições previsíveis entre as fases amarelo, vermelho e verde com intervalos ajustáveis, permitindo fácil extensão (botões, sensores, EEPROM) e montagem simples em protoboard com jumpers e resistores.

## Vídeo de demonstração:

https://youtube.com/shorts/m4Vm-y9-5EY

## Código do arduino:

```cpp
const unsigned long intervaloLeitura = 2000; 

unsigned long ultimoTempoLeitura = 0;
short int state = 1; // 1: Amarelo 2: Verde 3: Vermelho

short int ledVermelho = 9;
short int ledAmarelo = 11;
short int ledVerde = 13;

// Pointeiro para o led verde
int* ledAtual = &ledVerde; 

void setup() {
  pinMode(ledVermelho, OUTPUT);
  pinMode(ledAmarelo, OUTPUT);
  pinMode(ledVerde, OUTPUT);
  
  digitalWrite(*ledAtual, HIGH); // Liga o primeiro led
}

void loop()
{
  unsigned long tempoAtual = millis();

   /* 
     Intervalo de leitura depende do estado:
     1. Amarelo: 2000 
     2. Verde: 4000
     3. Vermelho: 6000
   */ 

   if (tempoAtual - ultimoTempoLeitura >= (intervaloLeitura * state)) {
      ultimoTempoLeitura = tempoAtual;  // Atualiza o último tempo de leitura

      digitalWrite(*ledAtual, LOW);
      Serial.println(*ledAtual);

      if (state == 1) {
        state = 3;
        ledAtual = &ledVermelho;  // Pointeiro para o vermelho
      } else if (state == 2) {
        state = 1;
        ledAtual = &ledAmarelo;   // Pointeiro para o Amarelo
      } else if (state == 3) {
        state = 2;
        ledAtual = &ledVerde;     // Pointeiro para o Verde
      }

      digitalWrite(*ledAtual, HIGH);
    }
}
```

## Bill of material

| Item                                       | Quantidade | Observações                                    |
| ------------------------------------------ | ---------: | ---------------------------------------------- |
| Arduino Uno                                |          1 | -                                              |
| Protoboard (breadboard)                    |          1 | Para montagem dos componentes                  |
| LED (5 mm, cores à sua escolha)            |         3x | Use um resistor por LED                        |
| Jumpers (macho-macho ou conforme montagem) |         9x | Cabos para conexões                            |
| Cabo USB tipo A                            |          1 | Cabo para alimentação/programação (USB tipo A) |
| Resistores (220 ohms , 1/4 W recomendado)  |         3x | 220 ohms é recomendado para LEDs em 5V         |

## Avaliações

### Avaliador 1:

Gabriel Bartmanovicz

| Critério                                                                                                            | Contempla (Pontos) | Contempla Parcialmente (Pontos) | Não Contempla (Pontos) | Observações do Avaliador |
| ------------------------------------------------------------------------------------------------------------------- | -----------------: | ------------------------------: | ---------------------: | ------------------------ |
| Montagem física com cores corretas, boa disposição dos fios e uso adequado de resistores                            |              Até 3 |                         Até 1,5 |                      0 |   Nota: 3                       |
| Temporização adequada conforme tempos medidos com auxílio de algum instrumento externo                              |              Até 3 |                         Até 1,5 |                      0 |    Nota: 3                           |
| Código implementa corretamente as fases do semáforo e estrutura do código (variáveis representativas e comentários) |              Até 3 |                         Até 1,5 |                      0 |   Nota: 3                            |
| Ir além: Implementou um componente extra, usou millis() ao invés do delay() e/ou utilizou ponteiros no código       |              Até 1 |                         Até 0,5 |                      0 |      Nota: 1                        |
| **Pontuação Total**                                                                                                 |                 10 |                                 |                        |                          |

### Avaliador 2:

Thulio Bacco

| Critério                                                                                                            | Contempla (Pontos) | Contempla Parcialmente (Pontos) | Não Contempla (Pontos) | Observações do Avaliador |
| ------------------------------------------------------------------------------------------------------------------- | -----------------: | ------------------------------: | ---------------------: | ------------------------ |
| Montagem física com cores corretas, boa disposição dos fios e uso adequado de resistores                            |              Até 3 |                         Até 1,5 |                      0 |  Nota: 3                            |
| Temporização adequada conforme tempos medidos com auxílio de algum instrumento externo                              |              Até 3 |                         Até 1,5 |                      0 |           Nota: 3                    |
| Código implementa corretamente as fases do semáforo e estrutura do código (variáveis representativas e comentários) |              Até 3 |                         Até 1,5 |                      0 |         Nota: 3                      |
| Ir além: Implementou um componente extra, usou millis() ao invés do delay() e/ou utilizou ponteiros no código       |              Até 1 |                         Até 0,5 |                      0 |      Nota: 1                        |
| **Pontuação Total**                                                                                                 |                 10 |                                 |                        |                          |
