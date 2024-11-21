# GS-EC
Monitor de Consumo de Energia com ESP32 e MQTT

Este projeto aborda o monitoramento de consumo energético em tempo real, utilizando o microcontrolador ESP32, um display LCD e comunicação MQTT. A solução é projetada para promover a eficiência energética em sistemas baseados em fontes renováveis, integrando-se com plataformas IoT como o FIWARE.

Problema Abordado
Com a crescente adoção de energias renováveis, há a necessidade de monitorar o consumo energético em tempo real, identificar padrões de uso e alertar sobre situações de alto consumo, promovendo uma utilização mais eficiente e sustentável da energia.

Solução Proposta
O sistema:
- Monitora o consumo energético em tempo real (em kWh) usando um potenciômetro para simular variações de carga.
- Exibe os dados no display LCD 20x4, com status (Bom, Elevado, Alto).
- Envia as informações via MQTT para um broker, permitindo integração com plataformas como FIWARE para análise remota e ações inteligentes.

Componentes Utilizados
- ESP32: Microcontrolador para processamento e envio de dados.
- Potenciômetro: Simula o consumo energético.
- LCD 20x4 com I2C: Exibe consumo em kWh e status.
- Broker MQTT: Recebe dados publicados pelo ESP32.

#include <Wire.h>
#include <LiquidCrystal_I2C.h>

// Definição do LCD (20x4)
LiquidCrystal_I2C lcd(0x27, 20, 4);  // Confirme o endereço I2C do LCD

// Definições dos pinos
const int potPin = 34;  // Pino do potenciômetro (ADC)

// Parâmetros para o cálculo
const float maxPotValue = 4095.0;  // Valor máximo do ADC
const float maxPower = 4400.0;      // Potência máxima simulada (em watts)

// Variáveis
float power = 0.0;          // Potência calculada
float energyConsumed = 0.0; // Energia consumida acumulada em kWh
unsigned long lastUpdateTime = 0;  // Último tempo atualizado (em ms)

// Intervalo para atualização (1 segundo = 1000 ms)
const unsigned long updateInterval = 1000;

// Função para avaliar o consumo
String avaliarConsumo(float energia) {
  if (energia < 0.5) {
    return "Bom";
  } else if (energia < 1.5) {
    return "Elevado";
  } else {
    return "Alto";
  }
}

void setup() {
  // Inicializa Serial e LCD
  Serial.begin(115200);
  lcd.init();
  lcd.backlight();

  // Mensagem inicial no LCD
  lcd.setCursor(0, 0);
  lcd.print("Simulador Iniciado");
  delay(2000);
  lcd.clear();
}

void loop() {
  // Atualiza a cada 1 segundo
  unsigned long currentTime = millis();
  if (currentTime - lastUpdateTime >= updateInterval) {
    lastUpdateTime = currentTime;

    // Leitura do potenciômetro
    int potValue = analogRead(potPin);

    // Cálculo da potência (em watts)
    power = (potValue / maxPotValue) * maxPower;

    // Calcula energia consumida (em kWh) no intervalo
    energyConsumed += (power / 1000.0) * (updateInterval / 3600000.0);

    // Avalia o consumo
    String statusConsumo = avaliarConsumo(energyConsumed);

    // Atualiza o LCD com as informações
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("Potencia: ");
    lcd.print(power, 1);  // Exibe potência com 1 casa decimal
    lcd.print(" W");

    lcd.setCursor(0, 1);
    lcd.print("Energia: ");
    lcd.print(energyConsumed, 3);  // Exibe energia com 3 casas decimais
    lcd.print(" kWh");

    lcd.setCursor(0, 2);
    lcd.print("Status: ");
    lcd.print(statusConsumo);  // Mostra se é Bom, Elevado ou Alto

    lcd.setCursor(0, 3);
    lcd.print("Atualizando...");

    // Simula envio MQTT no Serial Monitor
    Serial.print("Potencia (W): ");
    Serial.print(power, 1);
    Serial.print(", Energia (kWh): ");
    Serial.print(energyConsumed, 3);
    Serial.print(", Status: ");
    Serial.println(statusConsumo);
  }
}
