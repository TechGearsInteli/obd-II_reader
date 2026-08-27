---
title: Ecossistema OBD-II e Rede CAN
slug: /obd-ii
sidebar_position: 3
---

# Ecossistema OBD-II e Rede CAN

<div style={{textAlign: 'justify'}}>

**Responsável:** Equipe Techgears

**Objetivo:** Centralizar o conhecimento sobre o funcionamento do padrão OBD-II, a pinagem do conector DLC, a arquitetura de ECUs, o protocolo CAN Bus e a interface com o módulo ELM327 — servindo como material de referência para qualquer integrante do time, inclusive quem nunca teve contato com o assunto.

**Escopo:** Fundamentos de OBD-II, CAN Bus, DTCs, PIDs e uso prático do módulo ELM327.

## 1. Introdução

### 1.1 O que é o OBD-II

O **OBD-II** (*On-Board Diagnostics II*, ou "Diagnóstico a Bordo II") é um padrão industrial de diagnóstico veicular. De forma geral, ele funciona recebendo impulsos elétricos gerados pelos diversos sistemas do carro, identificando o que cada um representa e convertendo-os em informações legíveis sobre as condições reais do veículo.

Na prática, o OBD-II é a ponte entre a eletrônica interna do carro — sensores, módulos e fiação — e uma pessoa ou sistema externo que precisa entender o que está acontecendo com o veículo, seja um mecânico com um scanner de bancada, seja um microcontrolador embarcado em um projeto próprio, como um dashboard veicular.

### 1.2 Histórico e Evolução

O dispositivo surgiu inicialmente com a proposta de controlar e regular a emissão de gases poluentes decorrentes de falhas em sistemas do motor. A ideia original era puramente ambiental: garantir que problemas mecânicos que aumentassem a poluição fossem identificados rapidamente.

Com o tempo, o padrão foi sendo atualizado e passou a identificar não somente falhas voltadas à poluição, mas problemas de praticamente qualquer natureza no veículo — elétricos, mecânicos ou de comunicação entre módulos. Desde 1996, nos Estados Unidos, todo veículo de passeio é obrigado a possuir uma porta OBD-II compatível, e o padrão se popularizou mundialmente nos anos seguintes, tornando-se uma ferramenta universal de diagnóstico a bordo.

## 2. Conector de Link de Dados (DLC)

### 2.1 Visão Geral

O dispositivo exige uma conexão física de **16 pinos**, com um esquema padrão apresentado nas imagens desta seção. O **DLC** (*Data Link Connector*, ou "Conector de Link de Dados") é a porta física de 16 pinos do carro.

Embora o formato do plugue seja idêntico em todos os carros produzidos após 1996, os pinos internos efetivamente conectados variam dependendo do protocolo de comunicação que cada montadora escolheu usar. Ou seja: o encaixe é sempre o mesmo, mas nem todo pino carrega sinal em todo veículo — por isso, saber identificar quais pinos estão ativos é o primeiro passo de qualquer diagnóstico.

### 2.2 Localização no Veículo

O conector normalmente fica localizado no compartimento do motorista, próximo à coluna de direção, ao alcance da mão e sem necessidade de ferramentas para acessá-lo. A imagem abaixo destaca os locais mais comuns onde ele pode ser encontrado, variando conforme o modelo do veículo.

<div align="center">

<small><strong>Figura 1 - Localização típica do conector OBD-II no painel do veículo</strong></small>

![Localização típica do conector OBD-II no painel do veículo](/img/obd-ii/localizacao-conector-obd-ii.jpg)

<small>Fonte: Imagem incorporada à documentação original.</small>

</div>

### 2.3 Pinagem Padrão (SAE J1962)

A tabela a seguir apresenta a pinagem oficial do conector segundo a norma **SAE J1962**, que define a posição física de cada pino independentemente do protocolo utilizado pela montadora.

<div align="center">

<small><strong>Quadro 1 - Pinagem padrão do conector DLC</strong></small>

| **Pino** | **Descrição** | **Função / Protocolo** |
| --- | --- | --- |
| 1 | OEM / Fabricante | Variável (ex.: CAN de baixa velocidade em algumas montadoras) |
| 2 | J1850 Bus+ | Linha positiva do protocolo SAE J1850 (PWM/VPW) |
| 3 | OEM / Fabricante | Conexões específicas de fábrica / linhas de diagnóstico adicionais |
| 4 | Chassis Ground | Aterramento do chassi do veículo |
| 5 | Signal Ground | Aterramento do sinal lógico de comunicação |
| 6 | CAN High | Linha de alta velocidade da rede CAN (ISO 15765-4) |
| 7 | K-Line | Linha K para diagnóstico (ISO 9141-2 / ISO 14230-4) |
| 8 | OEM / Fabricante | Uso específico (ex.: verificação de sincronismo / ignição) |
| 9 | OEM / Fabricante | Uso específico (ex.: tacômetro / ECM) |
| 10 | J1850 Bus- | Linha negativa do protocolo SAE J1850 (apenas PWM) |
| 11 | OEM / Fabricante | Uso específico (ex.: atuador do freio / ABS) |
| 12 | OEM / Fabricante | Variável (ex.: CAN secundária do chassi +) |
| 13 | OEM / Fabricante | Variável (ex.: CAN secundária do chassi -) |
| 14 | CAN Low | Linha de baixa velocidade da rede CAN (ISO 15765-4 / SAE J2284) |
| 15 | L-Line | Linha L para ativação do diagnóstico (ISO 9141-2 / ISO 14230-4) |
| 16 | Battery Power (B+) | Alimentação permanente direto da bateria (+12V) |

<small>Fonte: elaborado pela equipe Techgears.</small>

</div>
### 2.4 Pinos Essenciais para Diagnóstico

Para a grande maioria dos projetos com rede CAN (a mais comum em carros atuais), os pinos que realmente importam são:

- **Pinos 4 e 5** → aterramento (ground);

- **Pino 6** → rede CAN Alta (CAN High);

- **Pino 14** → rede CAN Baixa (CAN Low);

- **Pino 16** → alimenta o scanner com +12V direto da bateria.

<div align="center">

<small><strong>Figura 2 - Esquema simplificado dos pinos essenciais do conector OBD-II</strong></small>

![Esquema simplificado dos pinos essenciais do conector OBD-II](/img/obd-ii/pinos-essenciais-obd-ii.gif)

<small>Fonte: Elaborado pela equipe Techgears.</small>

</div>

:::tip Dica de bancada
Os pinos fundamentais para ligar qualquer scanner genérico são o 4 e 5 (aterramento), o 16 (alimentação de +12V) e o par de comunicação da rede do veículo — geralmente 6 e 14 para carros modernos com rede CAN.
:::

### 2.5 Exemplos de Pinagem em Veículos Reais

Como cada montadora pode usar pinos diferentes para funções específicas, é útil observar exemplos reais de documentação técnica. Abaixo estão dois exemplos de referência: um mapeamento de cores usado em veículos Toyota e uma ficha técnica de bancada (conector X84) usada em treinamentos automotivos.

<div align="center">

<small><strong>Figura 3 - Exemplo de pinagem em veículo Toyota</strong></small>

![Exemplo de pinagem em veículo Toyota](/img/obd-ii/pinagem-toyota.gif)

<small>Fonte: Imagem incorporada à documentação original.</small>

</div>

<div align="center">

<small><strong>Figura 4 - Ficha técnica de bancada para o conector X84</strong></small>

![Ficha técnica de bancada para o conector X84](/img/obd-ii/conector-x84-simplotech.gif)

<small>Fonte: Simplotech.</small>

</div>

## 3. Arquitetura da Rede e Fluxo de Dados

### 3.1 Unidades de Controle Eletrônico (ECUs)

As **ECUs** (*Electronic Control Units*, ou Unidades de Controle Eletrônico) podem se referir a um único módulo ou a um conjunto de módulos. Elas são o "cérebro" do veículo, monitorando e controlando diversas funções. Em carros novos, são compostas por microcontroladores interconectados em rede, cada um responsável por um domínio específico do veículo. Os tipos mais comuns incluem:

- **ECM — Módulo de Controle do Motor (Engine Control Module):** controla os atuadores do motor, afetando aspectos como o ponto de ignição, a relação ar/combustível e a marcha lenta.

- **VCM — Módulo de Controle do Veículo (Vehicle Control Module):** outro nome comum para o módulo que gerencia o motor e o desempenho geral.

- **TCM — Módulo de Controle da Transmissão (Transmission Control Module):** gerencia o câmbio, monitorando a temperatura do fluido de transmissão, a posição do acelerador e a velocidade das rodas.

- **PCM — Módulo de Controle do Trem de Força (Powertrain Control Module):** unidade combinada que engloba o ECM e o TCM em um único hardware.

- **EBCM — Módulo de Controle Eletrônico de Freios (Electronic Brake Control Module):** controla e lê os dados do sistema de freios ABS.

- **BCM — Módulo de Controle da Carroceria (Body Control Module):** controla funções de conforto e cabine, como vidros elétricos, travas, alarmes e iluminação interna.

### 3.2 Do Sensor ao Scanner: o Fluxo de Comunicação

As ECUs convertem os sinais analógicos e físicos captados pelos sensores em bits por meio do **Conversor Analógico-Digital (ADC)**. Esses dados são então enviados a outras ECUs por meio do **CAN Bus** — o sistema que efetua a comunicação entre as ECUs, empacotando a informação em pacotes chamados Frames CAN (contendo dados como ID, conteúdo da mensagem e CRC, detalhados na seção 4).

O fluxo exato do momento em que um scanner é conectado é o seguinte:

1. **O scanner pede (requisição):** você pluga o scanner na tomada OBD-II. Ele envia uma pergunta pela rede (ex.: "qual é a rotação atual do motor?").

2. **O CAN Bus leva a pergunta:** essa requisição viaja pelos fios do CAN Bus até a ECU do motor.

3. **A ECU processa:** a ECU do motor recebe o pedido, olha para os bits que acabou de converter do sensor físico e separa a resposta.

4. **O CAN Bus traz a resposta:** a ECU joga os bits da resposta de volta no CAN Bus.

5. **O OBD-II entrega o resultado:** os bits chegam até o conector OBD-II, e o scanner traduz esses bits em algo legível (ex.: 1500 RPM) na tela.

<div align="center">

<small><strong>Figura 5 - Fluxo dos dados do sensor físico até a leitura no scanner</strong></small>

![Fluxo dos dados do sensor físico até a leitura no scanner](/img/obd-ii/fluxo-sensor-scanner.gif)

<small>Fonte: Elaborado pela equipe Techgears.</small>

</div>

### 3.3 Processamento Interno no Módulo OBD-II

Dentro do próprio OBD-II, o processamento de cada requisição segue três etapas:

1. Recepção dos dados binários;

2. Conversão para código hexadecimal;

3. Aplicação do dicionário de regras (PID) — explicado em detalhe na seção 7.

```text
[ Sensor do Carro ]
        | (gera sinal analógico)
        v
[ Central Eletrônica / ECU ]
        | (transforma em código)
        v
[ Protocolo CAN Bus ] -> Traduz impulsos elétricos em bytes
        |
        v
[ Padrão OBD-II ] -> Traduz os bytes em informação real
        |
        v
[ Scanner / OBD-Pi ] -> Exibe o valor para leitura
```
### 3.4 Validação de Bit-rate e Protocolo (CAN ID)

Como diferentes montadoras usam velocidades de comunicação (bit-rates) e identificadores diferentes, o dispositivo de diagnóstico precisa primeiro descobrir qual combinação o veículo está usando antes de conseguir conversar com ele. O processo, de forma resumida, consiste em testar cada combinação possível de bit-rate e CAN ID, enviando uma requisição padrão e observando se há resposta válida, erro de comunicação ou nenhuma resposta — repetindo o teste até encontrar a combinação correta.

<div align="center">

<small><strong>Figura 6 - Fluxo de validação de bit-rate e CAN ID</strong></small>

![Fluxo de validação de bit-rate e CAN ID](/img/obd-ii/validacao-bitrate-can-id.gif)

<small>Fonte: Imagem incorporada à documentação original.</small>

</div>

## 4. Fundamentos da Rede CAN Bus

### 4.1 A Analogia da Sala de Reunião

Imagine uma empresa antiga em que, para o Diretor de Vendas falar com o Diretor de Compras, era necessário passar um fio de telefone exclusivo entre as duas salas. Se o Diretor de Marketing também quisesse falar com eles, seriam necessários mais fios dedicados. Em um carro antigo era assim: cada sensor tinha um fio próprio indo para cada módulo que precisava dele. O chicote elétrico resultante era pesado, complexo e caro.

O **CAN Bus** (*Controller Area Network*) mudou isso. Ele funciona como uma sala de reunião com um microfone central:

- Todos os módulos do carro (injeção, ABS, airbag, painel) estão sentados nessa mesma sala.

- Existem apenas dois fios centrais (como o cabo do microfone que passa por toda a mesa).

- Quando o módulo do ABS quer informar a velocidade das rodas, ele simplesmente "fala no microfone".

- Todos os outros módulos escutam ao mesmo tempo: a injeção usa esse dado para controlar o motor, o painel usa para mover o ponteiro do velocímetro, e o airbag fica sabendo se o carro parou bruscamente. Quem não precisa daquela informação (como o ar-condicionado) simplesmente a ignora.

### 4.2 Estrutura de um Frame CAN

Como todos os módulos compartilham os mesmos dois fios, eles não podem falar ao mesmo tempo de qualquer jeito, senão vira bagunça. Para organizar a conversa, os dados são enviados em pacotes padronizados chamados **Frames CAN** (ou quadros de dados). Pense no Frame CAN como uma carta de correio que viaja pela rede: você não precisa decorar os bits, mas é importante saber que ele tem três partes principais:

- **ID (identificador / prioridade):** é o "assunto" da mensagem (ex.: "dados do freio ABS"). No CAN Bus, quanto menor o número do ID, maior a sua prioridade. Se o ABS (ID baixo) e o rádio (ID alto) tentarem falar ao mesmo tempo, a rede corta o rádio e deixa o ABS passar primeiro.

- **Data Field (campo de dados):** é o conteúdo real da mensagem, com tamanho máximo de 8 bytes no CAN tradicional. É aqui que viajam os valores, como a rotação do motor (RPM) ou a temperatura do óleo.

- **CRC (Cyclic Redundancy Check — checagem de erros):** um pequeno código matemático no final da mensagem. Se o pacote sofrer alguma interferência elétrica no caminho, o módulo que recebe a mensagem faz a conta, percebe que o CRC não bate e descarta a mensagem, solicitando o reenvio.

<div align="center">

<small><strong>Figura 7 - Estrutura simplificada de um frame CAN</strong></small>

![Estrutura simplificada de um frame CAN](/img/obd-ii/estrutura-frame-can.gif)

<small>Fonte: Elaborado pela equipe Techgears.</small>

</div>

### 4.3 Resistores de Terminação de 120 Ω

A rede CAN é composta por dois fios trançados: o **CAN High** e o **CAN Low**. Os sinais elétricos viajam por esses fios em altíssima velocidade — até 500 kbps ou mais.

Quando a onda elétrica chega ao fim do fio físico da rede, se encontrar uma "rua sem saída" (um fio cortado ou aberto), essa onda bate na ponta e reflete de volta pelo fio, gerando eco e interferência (ruído elétrico). Isso corrompe os dados e faz os módulos pararem de se comunicar corretamente.

Para evitar isso, coloca-se um resistor de 120 Ω em cada uma das duas extremidades físicas da rede, totalizando dois resistores em paralelo na rede toda. A função deles é agir como "amortecedores" para a energia elétrica: quando o sinal chega ao fim da linha, o resistor absorve a onda, impedindo que ela reflita de volta.

:::tip Dica prática de diagnóstico
Como os dois resistores de 120 Ω estão em paralelo nas pontas da rede, pela lei de Ohm eles resultam em uma resistência equivalente de 60 Ω. Com o veículo desligado e a bateria desconectada, meça a resistência entre o pino 6 (CAN High) e o pino 14 (CAN Low) usando um multímetro: resultado ideal próximo de 60 Ω indica rede íntegra; resultado de 120 Ω indica circuito aberto em uma das pontas da rede.
:::

## 5. Protocolos de Comunicação OBD-II

### 5.1 Os Cinco Protocolos Homologados

Atualmente existem cinco protocolos de comunicação homologados pelo padrão OBD-II, cada um mais comum em um grupo de montadoras e período de fabricação:

<div align="center">

<small><strong>Quadro 2 - Protocolos de comunicação homologados pelo padrão OBD-II</strong></small>

| **Protocolo** | **Características** | **Aplicação principal** |
| --- | --- | --- |
| ISO 15765-4 (CAN) | Alta velocidade, baseada em barramento de dois fios | Maioria absoluta dos carros atuais (obrigatório desde 2008) |
| ISO 14230-4 (KWP2000) | Baseado em Linha K, evolução do ISO 9141 | Comum em veículos asiáticos e europeus pós-2003 |
| ISO 9141-2 | Linha K de comunicação serial simples | Veículos europeus, Chrysler e asiáticos (2000-2004) |
| SAE J1850 VPW | Modulação por largura de pulso variável (1 fio) | Principalmente veículos antigos da GM |
| SAE J1850 PWM | Modulação por largura de pulso diferencial (2 fios) | Principalmente veículos antigos da Ford |

<small>Fonte: elaborado pela equipe Techgears.</small>

</div>
<div align="center">

<small><strong>Figura 8 - Comparativo visual dos cinco protocolos compatíveis com OBD-II</strong></small>

![Comparativo visual dos cinco protocolos compatíveis com OBD-II](/img/obd-ii/protocolos-obd-ii.gif)

<small>Fonte: Iaranja, conforme indicado na documentação original.</small>

</div>

### 5.2 Pinagem Detalhada por Protocolo

O diagrama abaixo consolida, sobre o desenho do conector de 16 pinos, quais pinos correspondem a cada protocolo — útil como referência rápida ao identificar qual rede um veículo específico utiliza.

<div align="center">

<small><strong>Figura 9 - Pinagem consolidada por protocolo OBD-II</strong></small>

![Pinagem consolidada por protocolo OBD-II](/img/obd-ii/pinagem-por-protocolo.gif)

<small>Fonte: Elaborado pela equipe Techgears.</small>

</div>

## 6. Códigos de Diagnóstico de Falhas (DTC)

Os **DTCs** (*Diagnostic Trouble Codes*) são usados para descrever onde um problema está ocorrendo no veículo, sendo padronizados pela **SAE** (*Society of Automotive Engineers*). Eles podem ser genéricos (iguais para todo carro) ou específicos de cada fabricante.

Os códigos seguem sempre o formato de 5 dígitos (XXXXX), em que cada posição tem um significado específico:

### 6.1 1º Dígito (Letra) — Sistema Principal

- **P**xxxx: *Powertrain* (trem de força / motor / câmbio)

- **B**xxxx: *Body* (carroceria / chassi interno / airbag)

- **C**xxxx: *Chassis* (chassi externo / freios / suspensão)

- **U**xxxx: *Network* (rede de comunicação entre módulos)

### 6.2 2º Dígito — Origem da Padronização

- x**0**xxx: código genérico, regulamentado pelo governo (SAE).

- x**1**xxx: código específico da montadora.

### 6.3 3º Dígito — Subsistema Afetado

- xx**1**xx / xx**2**xx: medição de ar e combustível.

- xx**3**xx: sistema de ignição ou falha de centelha (*misfire*).

- xx**4**xx: sistemas de controle de emissões poluentes.

- xx**5**xx: controle de velocidade do veículo e marcha lenta.

- xx**6**xx: computador de bordo e circuitos de saída.

- xx**7**xx / xx**8**xx: transmissão (câmbio).

- xx**9**xx: sinais de entrada/saída e controles gerais.

### 6.4 4º e 5º Dígitos — Falha Específica

Os dois últimos dígitos (xx**XX**) identificam a falha específica, variando de 00 a 99 e detalhando exatamente qual componente ou circuito está operando fora dos parâmetros nominais.

## 7. PIDs (Parameter IDs)

### 7.1 O que são PIDs

**PID** significa *Parameter Identification* (Identificação de Parâmetro). São códigos hexadecimais de requisição que enviamos para a ECU do veículo pedindo um dado específico.

Quando o scanner (ou o sistema desenvolvido pela equipe) envia um frame CAN perguntando por um PID específico, o carro responde com um pacote contendo bytes de dados "brutos". Cabe ao software da equipe pegar esses bytes e aplicar uma fórmula matemática específica para transformá-los em valores compreensíveis, como RPM, °C ou km/h. Todos os PIDs padrão utilizados neste documento pertencem ao **Modo 01** (mostrar dados atuais do diagnóstico).

### 7.2 Tabela de PIDs Selecionados para o Projeto (V1)

Para a Versão 1 (V1) do dashboard da equipe, o foco está nos dados vitais do motor, marcados como prioridade crítica ou alta. Os demais PIDs ficam mapeados para atualizações futuras (V2).

<div align="center">

<small><strong>Quadro 3 - PIDs selecionados para o projeto</strong></small>

| **PID (Hex)** | **Parâmetro** | **Unidade** | **Fórmula** | **Prioridade** |
| --- | --- | --- | --- | --- |
| 0C | Engine RPM — rotações por minuto do motor | rpm | (256×A + B) / 4 | V1 (Crítica) |
| 0D | Vehicle Speed — velocidade atual do veículo | km/h | A | V1 (Crítica) |
| 05 | Engine Coolant Temp — temperatura do fluido de arrefecimento | °C | A − 40 | V1 (Crítica) |
| 11 | Throttle Position — posição da borboleta de aceleração | % | 100×A / 255 | V1 (Alta) |
| 0F | Intake Air Temp — temperatura do ar na admissão | °C | A − 40 | V2 (Média) |
| 04 | Calculated Engine Load — carga calculada do motor | % | 100×A / 255 | V2 (Média) |
| 2F | Fuel Tank Level Input — nível de combustível do tanque | % | 100×A / 255 | V2 (Média) |

<small>Fonte: elaborado pela equipe Techgears.</small>

</div>

:::note Variáveis A e B
Nas fórmulas acima, A representa o primeiro byte de dados retornado pelo carro, e B representa o segundo byte, quando houver.
:::

### 7.3 Referência Completa de PIDs

A lista completa de PIDs padronizados (muito mais ampla que a tabela V1/V2 acima) está documentada na Wikipedia: [en.wikipedia.org/wiki/OBD-II\_PIDs](https://en.wikipedia.org/wiki/OBD-II_PIDs).

## 8. Módulo ELM327

### 8.1 O que é e como Funciona

O **ELM327** é um chip/módulo que atua como **tradutor** entre a porta de diagnóstico do carro (OBD-II) e qualquer microcontrolador ou computador. Ele entende dois "idiomas":

<div align="center">

<small><strong>Quadro 4 - Linguagens utilizadas pelo módulo ELM327</strong></small>

| **Lado** | **Linguagem** |
| --- | --- |
| Computador / ESP32 | Comandos AT (texto simples via UART/serial) |
| Carro | Protocolos OBD-II (CAN, ISO, PWM etc.) |

<small>Fonte: elaborado pela equipe Techgears.</small>

</div>
Um chip com circuitos internos conectados a quase todos os pinos da tomada OBD-II. A primeira coisa que ele faz, ao ligar a chave do carro, é verificar em quais pinos há atividade elétrica ou variação de tensão. Em seguida, por meio do envio de uma mensagem de teste, ele aguarda uma resposta do CAN e, dependendo de quais pinos responderem, consegue identificar automaticamente qual protocolo está sendo utilizado pelo veículo.

Na prática, você envia um comando AT em texto, e o ELM327 se comunica com o carro e devolve a resposta.

```text
Enviar:  010C
Receber: 41 0C 1A F8
```
### 8.2 Configuração da Conexão Serial (UART)

Para interagir diretamente com o módulo usando um terminal de texto (como PuTTY, Arduino Serial Monitor, screen ou minicom), configure os seguintes parâmetros:

```text
Baud rate: 38400 (padrão ELM327; alguns módulos usam 9600 ou 115200)
Data bits:  8
Stop bits:  1
Parity:     None
Flow ctrl:  None
```
### 8.3 Terminal Recomendado por Sistema Operacional

<div align="center">

<small><strong>Quadro 5 - Terminais recomendados por sistema operacional</strong></small>

| **Sistema Operacional** | **Terminal recomendado** |
| --- | --- |
| Windows | PuTTY ou Arduino IDE (Monitor Serial) |
| Linux | screen ou minicom |
| Mac | screen, no Terminal |

<small>Fonte: elaborado pela equipe Techgears.</small>

</div>
### 8.4 Conectando via Terminal — Linux/Mac

```bash
# Descobrir a porta
ls /dev/tty.*       # macOS
ls /dev/ttyUSB*     # Linux

# Conectar
screen /dev/ttyUSB0 38400
```
### 8.5 Conectando via Terminal — Windows

1. Abrir o **Gerenciador de Dispositivos** e verificar qual porta COM aparece.

2. Abrir o **PuTTY** e selecionar Connection type: **Serial**.

3. Preencher a porta (ex.: COM3) e o baud rate 38400.

### 8.6 Sequência de Inicialização (Comandos AT)

Assim que conectado, envie os comandos abaixo em ordem, um por vez, pressionando Enter após cada um. A resposta esperada para todos, com exceção do reset, é OK:

<div align="center">

<small><strong>Quadro 6 - Sequência de inicialização do módulo ELM327</strong></small>

| **Comando** | **O que faz** | **Resposta esperada** | **Observação** |
| --- | --- | --- | --- |
| ATZ | Reset geral do módulo | ELM327 vX.X (ou similar) | Reinicia o firmware do integrador |
| ATE0 | Desliga o eco dos comandos | OK | Evita que o ELM327 repita o texto enviado |
| ATL0 | Desliga line feeds (\n) | OK | Compacta o retorno, limpando quebras de linha |
| ATH1 | Ativa a exibição de headers | OK | Útil para visualizar os bytes de ID da resposta |
| ATSP0 | Seleção automática de protocolo | OK | O ELM327 varre os pinos para achar a rede ativa |

<small>Fonte: elaborado pela equipe Techgears.</small>

</div>
### 8.7 Leitura de Dados em Tempo Real e Diagnóstico (Modo 01)

Com o carro ligado (motor rodando), é possível solicitar diversos parâmetros. A tabela abaixo resume os principais comandos, incluindo os de diagnóstico:

<div align="center">

<small><strong>Quadro 7 - Comandos de leitura e diagnóstico</strong></small>

| **Comando** | **Parâmetro / função** | **Bytes** | **Fórmula** | **Exemplo** |
| --- | --- | --- | --- | --- |
| 0100 | PIDs suportados (01 a 20) | 4 bytes | Bitmap binário | 41 00 BE 3E 2F 11 |
| 010C | Rotação do motor (RPM) | 2 bytes (A, B) | (A×256 + B) / 4 | 41 0C 1A F8 → 1726 RPM |
| 010D | Velocidade do veículo (km/h) | 1 byte (A) | A | 41 0D 32 → 50 km/h |
| 0105 | Temp. do fluido de arrefecimento (°C) | 1 byte (A) | A − 40 | 41 05 6E → 70°C |
| 010F | Temp. do ar de admissão / IAT (°C) | 1 byte (A) | A − 40 | 41 0F 46 → 30°C |
| 0111 | Posição da borboleta de aceleração (%) | 1 byte (A) | A × 100 / 255 | 41 11 33 → 20% |
| 0104 | Carga calculada do motor (%) | 1 byte (A) | A × 100 / 255 | 41 04 7F → 49,8% |
| 012F | Nível de combustível do tanque (%) | 1 byte (A) | A × 100 / 255 | 41 2F CC → 80% |
| 0108 | Pressão absoluta no coletor / MAP (kPa) | 1 byte (A) | A | 41 08 64 → 100 kPa |
| 03 | Lê códigos de falha armazenados (DTCs) | Variável | Ver seção 6 | Retorna a lista de erros ativos |
| 04 | Limpa a memória de erros (apaga DTCs) | Nenhuma | — | OK (limpeza bem-sucedida) |
| 09 02 | Solicita o número VIN do veículo | Variável | Hex → ASCII | Retorna os caracteres alfanuméricos |

<small>Fonte: elaborado pela equipe Techgears.</small>

</div>

:::warning Nota importante de programação
Ao implementar o parser de temperatura (PIDs 0105 e 010F), use variáveis do tipo inteiro com sinal (`int` ou `int8_t`). Como a constante de subtração da fórmula é 40, qualquer byte de resposta menor que `0x28` (40 em decimal) gera um resultado de temperatura negativo — situação comum em testes em climas frios.
:::

### 8.8 Tabela de Conversão Hexadecimal → Decimal (Referência Rápida)

```text
0x0A = 10
0x1A = 26
0x32 = 50
0x6E = 110
0xF8 = 248
0xFF = 255
```
### 8.9 Fluxo Recomendado para Qualquer Leitura

1. Conectar via serial (38400 baud).

2. Enviar ATZ e aguardar resposta do módulo.

3. Enviar ATE0, ATL0, ATH1 e ATSP0.

4. Ligar o carro.

5. Enviar o PID desejado (ex.: 010C).

6. Extrair os bytes A e B da resposta.

7. Aplicar a fórmula correspondente ao PID.

## 9. Demonstração Prática: Convertendo RPM Manualmente

Para validar o algoritmo do software do painel/dashboard da equipe, é útil simular manualmente a resposta real de um carro e refazer a conversão à mão antes de automatizá-la em código. A seguir, dois exemplos independentes, com pacotes de bytes diferentes.

### 9.1 Exemplo 1 — Leitura Rápida

Requisição enviada: 010C. Resposta recebida:

```text
41 0C 1A F8
```
Para converter o resultado 1A F8 para RPM real:

```text
RPM = (A × 256 + B) / 4
RPM = (0x1A × 256 + 0xF8) / 4
RPM = (26 × 256 + 248) / 4
RPM = 6904 / 4
RPM = 1726 RPM
```
### 9.2 Exemplo 2 — Cálculo Detalhado Passo a Passo

Enviamos uma requisição para o PID 0C (RPM). O carro responde com o seguinte pacote de dados em formato hexadecimal:

```text
1A F4
```
Onde:

- Byte A = 1A (hexadecimal)

- Byte B = F4 (hexadecimal)

#### Passo 1 — Converter de hexadecimal para decimal

```text
Bytes recebidos: 41  0C  1A  F4
                       |   |
                       A   B  <- bytes utilizados
                   +-- confirma o PID

A = 1A(16) -> (1 × 16) + 10 = 26
B = F4(16) -> (15 × 16) + 4 = 244
```
#### Passo 2 — Aplicar a fórmula do RPM

A fórmula padrão do PID 0C é:

```text
Resultado = (256 × A + B) / 4

Substituindo os valores decimais encontrados:
Resultado = (256 × 26 + 244) / 4
Resultado = (6656 + 244) / 4
Resultado = 6900 / 4
Resultado = 1725 RPM
```

**Conclusão:** quando a ECU do carro devolveu os bytes 1A F4, o motor estava girando exatamente a 1725 RPM.

:::info Por que os dois exemplos têm resultados diferentes?
Os exemplos 9.1 e 9.2 usam pacotes de bytes ligeiramente diferentes (F8 no primeiro, F4 no segundo). Não se trata de um erro de cálculo, mas de duas capturas distintas da rede CAN, cada uma representando uma rotação diferente no momento da leitura.
:::

### 9.3 Observações para Implementação em Software

- Ao programar, trate os valores de temperatura com suporte a números negativos, já que a fórmula (A − 40) permite resultados abaixo de zero.

- O PID de velocidade (0D) usa apenas 1 byte (A) e não exige divisão — o valor decimal puro já é a velocidade em km/h.

## 10. Links e Referências

- Repositório de APIs OBD-II para Arduino / ESP32: [StanleyHuangYC - ArduinoOBD](https://github.com/stanleyhuangyc/ArduinoOBD)

- Documentação de referência de PIDs padrão: [Wikipedia — OBD-II PIDs](https://en.wikipedia.org/wiki/OBD-II_PIDs)

</div>
