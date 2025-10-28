# Semaforo offline

<img src="assets/image.jpg">

## Relatorio

Projeto de semáforo offline com Arduino Uno que reproduz as três fases (verde, amarelo e vermelho) usando três LEDs e temporizações controladas por millis(). O objetivo foi implementar um semáforo simples sem bloqueios (evitando delay()), garantindo transições previsíveis e permitindo fácil ajuste dos tempos de cada fase.

Hardware:
- Arduino Uno, 3 LEDs (verde, amarelo, vermelho) com resistores de 220 ohms em série.
- Conexões simples com jumpers; pinos digitais 9 (vermelho), 11 (amarelo) e 13 (verde) configurados como saída.

Software:
- Controle de tempo baseado em millis() para não bloquear a execução.
- Estado representado por variável inteira (state) que determina a fase atual.
- Intervalos dependem do estado: base de 2000 ms multiplicada pelo valor do estado (resultando em 2000 ms para amarelo, 4000 ms para verde e 6000 ms para vermelho).
- Transições realizadas apagando o LED anterior e acendendo o correspondente à nova fase.

Funcionamento observado:
- Ao ligar, o sistema inicia em estado definido (amarelo) e realiza ciclos contínuos entre amarelo → vermelho → verde → amarelo.
- Uso de millis() permite futura extensão (leitura de sensores, comunicação ou interrupções) sem afetar a temporização principal.

Testes e validação:
- Temporizações verificadas visualmente; recomenda-se medir com cronômetro ou osciloscópio para avaliação mais precisa.
- Testes de robustez devem incluir reinicialização, perda de alimentação e verificação de brilho/resistência dos LEDs.

Possíveis melhorias:
- Ajustar tempos por constantes nomeadas separadas (p.ex. TEMPO_VERDE, TEMPO_AMARELO, TEMPO_VERMELHO) para maior clareza.
- Implementar máquina de estados explícita com enum para legibilidade.
- Adicionar botão de pedestre, sensor de presença ou modo manual para enriquecer funcionalidade.
- Incluir debounce para entradas e persistência de configuração via EEPROM se necessário.

Conclusão:
Solução simples e funcional para demonstrar controle de fases de semáforo com Arduino sem bloqueios. O código é adequado para atividades didáticas e pode ser estendido facilmente para funcionalidades adicionais.
  

## Vídeo de demonstração:

https://youtube.com/shorts/u0INueWsoyM 

## Código do arduino:

```cpp
const unsigned long intervaloLeitura = 2000;

unsigned long ultimoTempoLeitura = 0;
short int state = 1; // 1: Amarelo 2: Verde 3: Vermelho

void setup()
{
  pinMode(9, OUTPUT); // Vermelho
  pinMode(11, OUTPUT); // Amarelo
  pinMode(13, OUTPUT); // Verde
  Serial.begin(9600);
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

        if(state == 1) {
          // Liga a led vermelha se a led anterior foi amarela
          state = 3;
          digitalWrite(11, LOW);
          digitalWrite(9, HIGH);
        } else if(state == 2) {
          // Liga a led amarela se a led anterior foi verde
          state = 1;
          digitalWrite(13, LOW);
          digitalWrite(11, HIGH);
        } else if(state == 3) {
          // Liga a led verde se a led anterior foi vermelha
          state = 2;
          digitalWrite(9, LOW);
          digitalWrite(13, HIGH);
        }
    }
}
```

## Bill of material

| Item                                       | Quantidade | Observações                         |
| ------------------------------------------ | ---------: | ----------------------------------- |
| Arduino Uno                                |          1 | -                                   |
| LED (5 mm, cores à sua escolha)            |         3x | Use um resistor por LED             |
| Jumpers (macho-macho ou conforme montagem) |         9x | Cabos para conexões                 |
| Resistores (220 ohms , 1/4 W recomendado)      |         3x | 220 ohms é recomendado para LEDs em 5V |


## Avaliações

### Avaliador 1:

Gabriel Bartmanovicz

| Critério                                                                                                            | Contempla (Pontos) | Contempla Parcialmente (Pontos) | Não Contempla (Pontos) | Observações do Avaliador |
| ------------------------------------------------------------------------------------------------------------------- | -----------------: | ------------------------------: | ---------------------: | ------------------------ |
| Montagem física com cores corretas, boa disposição dos fios e uso adequado de resistores                            |              Até 3 |                         Até 1,5 |                      0 |                          |
| Temporização adequada conforme tempos medidos com auxílio de algum instrumento externo                              |              Até 3 |                         Até 1,5 |                      0 |                          |
| Código implementa corretamente as fases do semáforo e estrutura do código (variáveis representativas e comentários) |              Até 3 |                         Até 1,5 |                      0 |                          |
| Ir além: Implementou um componente extra, usou millis() ao invés do delay() e/ou utilizou ponteiros no código       |              Até 1 |                         Até 0,5 |                      0 |                          |
| **Pontuação Total**                                                                                                 | 10                   |                                 |                        |                          |

### Avaliador 2:
Thulio Bacco

| Critério                                                                                                            | Contempla (Pontos) | Contempla Parcialmente (Pontos) | Não Contempla (Pontos) | Observações do Avaliador |
| ------------------------------------------------------------------------------------------------------------------- | -----------------: | ------------------------------: | ---------------------: | ------------------------ |
| Montagem física com cores corretas, boa disposição dos fios e uso adequado de resistores                            |              Até 3 |                         Até 1,5 |                      0 |                          |
| Temporização adequada conforme tempos medidos com auxílio de algum instrumento externo                              |              Até 3 |                         Até 1,5 |                      0 |                          |
| Código implementa corretamente as fases do semáforo e estrutura do código (variáveis representativas e comentários) |              Até 3 |                         Até 1,5 |                      0 |                          |
| Ir além: Implementou um componente extra, usou millis() ao invés do delay() e/ou utilizou ponteiros no código       |              Até 1 |                         Até 0,5 |                      0 |                          |
| **Pontuação Total**                                                                                                 | 10                   |                                 |                        |                          |