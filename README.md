# GS-EC
Rodrigo Akira Hirota Mori RM:555384
Gabriel Barros Mazzariol RM:555410
https://wokwi.com/projects/415036856656951297
https://youtu.be/7ARBGg3XU8g?si=My5r2Hvx8UMF7pq_

Monitor de Consumo de Energia com ESP32 e MQTT:
Este projeto aborda o monitoramento de consumo energético em tempo real, utilizando o microcontrolador ESP32, um display LCD e comunicação MQTT. A solução é projetada para promover a eficiência energética em sistemas baseados em fontes renováveis, integrando-se com plataformas IoT como o FIWARE.

Problema Abordado:
Com a crescente adoção de energias renováveis, há a necessidade de monitorar o consumo energético em tempo real, identificar padrões de uso e alertar sobre situações de alto consumo, promovendo uma utilização mais eficiente e sustentável da energia.

Solução Proposta:
O sistema:
- Monitora o consumo energético em tempo real (em kWh) usando um potenciômetro para simular variações de carga.
- Exibe os dados no display LCD 20x4, com status (Bom, Elevado, Alto).
- Envia as informações via MQTT para um broker, permitindo integração com plataformas como FIWARE para análise remota e ações inteligentes.

Componentes Utilizados:
- ESP32: Microcontrolador para processamento e envio de dados.
- Potenciômetro: Simula o consumo energético.
- LCD 20x4 com I2C: Exibe consumo em kWh e status.
- Broker MQTT: Recebe dados publicados pelo ESP32.

