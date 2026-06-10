---
title: Tranceiver CAN
slug: /CAN
sidebar_position: 5
---

# Transceiver CAN Bus

O CAN (*Controller Area Network*) é um padrão de comunicação feito para permitir que microcontroladores se comuniquem entre si sem a necessidade de uma conexão host. É um protocolo baseado em mensagens, amplamente utilizado em aplicações automotivas.

---

## Função

O Transceiver CAN Bus serve como intermediário entre o ESP32 e a elétrica do carro. O ESP32 não pode ser ligado diretamente nos fios do veículo, então o transceiver recebe os sinais digitais do ESP32, gera tensões mais altas e adequadas, e as envia para a rede elétrica do carro — permitindo assim a comunicação entre os dois.

Em resumo, ele traduz a linguagem eletrônica do ESP32 para a linguagem elétrica do carro, e vice-versa.

---

## Como funciona na prática

1. O ELM327 envia pacotes CAN com as informações do carro.
2. O Transceiver CAN recebe esses pacotes e os traduz para uma linguagem que o ESP32 consegue entender.
3. Os dados já traduzidos são enviados diretamente ao ESP32.