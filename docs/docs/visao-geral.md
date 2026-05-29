---
title: Visão Geral
slug: /
sidebar_position: 1
---

# OBD-II Reader

<div style={{textAlign: 'justify'}}>

&emsp;O **OBD-II Reader** é um dispositivo de leitura de dados veiculares de código aberto desenvolvido pela [TechGears](https://techgears.app), clube universitário do Inteli. O projeto tem como objetivo construir do zero um hardware compacto que se conecta à porta de diagnóstico padrão presente em todos os carros fabricados a partir de 1996 e expõe os dados do motor num dashboard web acessível pelo navegador do celular ou do notebook, sem instalar nenhum aplicativo.

&emsp;O dispositivo lê grandezas do motor em tempo real (rotação, temperatura, velocidade, posição da borboleta, mistura ar-combustível e outros), detecta e exibe os códigos de falha armazenados na ECU do veículo, registra os dados em cartão de memória para análise posterior e cria sua própria rede sem fio local para que qualquer dispositivo próximo possa se conectar diretamente, sem depender de roteador ou internet.

## O que você vai encontrar aqui

&emsp;Esta documentação cobre todo o projeto, do estudo inicial à validação em veículo real:

- **Hardware**: diagrama de blocos, lista de componentes e guia de montagem do circuito
- **Firmware**: arquitetura em camadas, driver OBD-II, comunicação sem fio e log em cartão de memória
- **Guia do usuário**: como conectar o dispositivo ao carro, acessar o dashboard e interpretar os dados
- **API**: referência dos endpoints e do canal de dados em tempo real
- **Troubleshooting**: problemas comuns e como resolvê-los
- **Diário de desenvolvimento**: registro das decisões e aprendizados ao longo do projeto

## Repositório

&emsp;O código-fonte, esquemáticos e demais arquivos estão em [github.com/TechGearsInteli/obd-II_reader](https://github.com/TechGearsInteli/obd-II_reader).

</div>
