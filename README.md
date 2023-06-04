# RTOS

<h2>Introdução ao RTOS</h2>
<p><strong>RTOS (Real-Time Operating System) é um sistema operacional projetado para aplicações que requerem tempo de resposta determinístico e previsível. Ao contrário de sistemas operacionais tradicionais, que são projetados para fornecer um ambiente de execução geral para vários tipos de aplicativos, um RTOS é otimizado para tarefas em tempo real, onde o tempo é um fator crítico.

Um RTOS é usado em uma variedade de sistemas embarcados, como sistemas de controle industrial, dispositivos médicos, sistemas de automação residencial, veículos autônomos e muitos outros. A principal característica de um RTOS é a capacidade de gerenciar tarefas com requisitos de tempo real, garantindo que elas sejam executadas dentro de prazos específicos.</strong</p>
 <hr></hr>
  </br>
  
  <h2>Multitarefa com FreeRTOS</h2>
  <p><strong>O FreeRTOS é um popular sistema operacional de tempo real (RTOS) de código aberto que suporta multitarefa em sistemas embarcados. Ele fornece um ambiente de execução para criar e gerenciar várias tarefas em um sistema embarcado</strong></p>
   </br>
   
   <h2>Sincronização de tarefas</h2>
    <p><strong>Poias a tarefas são feitas com poucos processo então não precisa listar o processo com prioridade</strong></p>
  <hr></hr>
  <p><strong>Semáforos e mutexes são mecanismos de exclusão mútua que ajudam a evitar condições de corrida, em que várias tarefas competem por recursos compartilhados simultaneamente, podendo levar a resultados inconsistentes ou indesejados. A criação de semáforos e mutexes em um ambiente de produção, como com o uso do FreeRTOS, é essencial para garantir a comunicação e sincronização adequadas entre as tarefas, evitando problemas de concorrência e garantindo o acesso seguro a recursos compartilhados.</strong></p>

![RTOS](https://github.com/tamis2K/RTOS/assets/90485488/9db91d8d-5887-4861-b491-97db7afc8374)


</br>
<h2>Código</h2>
  <hr></hr>

```javascript
#include <Arduino.h>
#include <Arduino_FreeRTOS.h>
#include <LiquidCrystal.h>

LiquidCrystal lcd(12,11,10,9,8,7);

#define LED_PIN_2 5
#define LED_PIN_1 4


void TaskReadTemperature(void *pvParameters);

volatile float temperature = 0.0;
void setup() {
 Serial.begin(9600);
 lcd.begin(16,2);

void TaskBlink1(void *pvParameters);
pinMode(LED_PIN_1, OUTPUT);

void TaskBlink2(void *pvParameters);
pinMode(LED_PIN_2, OUTPUT);

xTaskCreate(
  TaskBlink1, 
  "Blink1",
  128,   
  NULL,
  2, 
  NULL);

 xTaskCreate(
 TaskReadTemperature,
 "ReadTemperature",
 128,
 NULL,
 3,
 NULL );

 xTaskCreate(
  TaskBlink2, 
  "Blink2",
  128,   
  NULL,
  1, 
  NULL);

}

void loop() {
 // nada aqui!
}
void TaskReadTemperature(void *pvParameters) {
 (void) pvParameters;
 float sensorValue = 0.0;
 for (;;) {
 // Aqui você normalmente leria o valor do sensor de temperatura.
 // Por simplicidade, vamos apenas simular um sensor variando a
 sensorValue = -10.0 + (rand() % 51); // gera um número aleatório entre
 temperature = sensorValue;
 vTaskDelay(500 / portTICK_PERIOD_MS); // aguarda por 2 segundos
 lcd.setCursor(0,0);
 lcd.print("Temp: ");
 lcd.print(temperature);
 lcd.print(" C");
 // vTaskDelay(1000 / portTICK_PERIOD_MS);
 }
}
void TaskBlink1(void *pvParameters){
  (void) pvParameters;
  for (;;){
    digitalWrite(LED_PIN_1, HIGH);
    vTaskDelay(500 / portTICK_PERIOD_MS); 
    digitalWrite(LED_PIN_1, LOW); 
    vTaskDelay(500 / portTICK_PERIOD_MS);
  }
}

void TaskBlink2(void *pvParameters){
  (void) pvParameters;
  for (;;){
    if (temperature == 26){
    digitalWrite(LED_PIN_2, HIGH);
    }
  }
}
```
