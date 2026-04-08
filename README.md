# EcoTech — Lixeira inteligente (Firmware)

Este repositório contém o **firmware da lixeira ecológica inteligente EcoTech**, baseada em **ESP32-S3**, com **LCD + touch** e **célula de carga (load cell)** para medir o descarte em gramas.

> App (mobile) que se integra via Firebase/API: **igorfcfs/EcoTech**.

## Visão geral

A **EcoTech Soluções Ambientais Ltda.** é uma empresa dedicada a transformar o descarte de resíduos eletrônicos em uma experiência **inteligente, justa e sustentável**. A solução completa é composta por:

- **Lixeira inteligente** (este repositório): interface em LCD/touch, medição de massa (g) via load cell e envio do descarte para a API.
- **Aplicativo móvel** (outro repositório): acompanha descartes, pontos e benefícios, e integra com Firebase.

## Como funciona (fluxo resumido)

1. Usuário informa/valida o **CPF** na tela.
2. Seleciona a **categoria** do item (ex.: computador, celular, bateria, pilha).
3. A lixeira mede a **massa (g)** via HX711 + célula de carga.
4. O firmware envia um **POST** para a API (endpoint `/eletronicos`) com `cpf`, `categoria`, `massa` e `lixeiraDescarte`.

## Estrutura do repositório

- `Lixeira/Firmware/` — firmwares da lixeira (variações por hardware)
- `Lixeira/Esquema/` — esquemático (Fritzing) e referência de ligações

## Firmware: ESP32-S3 com LCD + Load Cell

Firmware principal (conforme o projeto):

- `Lixeira/Firmware/FIrmware ESP32S3 with lcd and loadcell/firmware/`

### Pré-requisitos

- Placa **ESP32-S3**
- Display **ILI9341** (TFT_eSPI) + **touch XPT2046**
- **Célula de carga** + módulo **HX711**
- Arduino IDE (ou outro fluxo compatível com `.ino`)

> Observação: este firmware é um sketch Arduino (`firmware.ino`).

### Configuração

1. Abra a pasta:

   `Lixeira/Firmware/FIrmware ESP32S3 with lcd and loadcell/firmware/`

2. Configure a rede Wi‑Fi, API e identificador da lixeira:

   - Copie `config_example.h` para `config.h`
   - Edite `config.h` com seus valores:
     - `ssid`
     - `password`
     - `serverBase` (base da API)
     - `idLixeira`

3. Ajuste o arquivo `User_Setup.h` (TFT_eSPI) se necessário:

   - driver: `ILI9341_DRIVER`
   - pinos do TFT e do touch (já definidos para ESP32‑S3)

### Ligações (pinos e hardware)

As ligações físicas devem seguir o esquemático em:

- `Lixeira/Esquema/Esquemático fritzing.fzz`

O firmware também define, por padrão:

- HX711:
  - `LOADCELL_DOUT_PIN = 27`
  - `LOADCELL_SCK_PIN = 26`

### Compilar e gravar

1. Abra `firmware.ino` na Arduino IDE.
2. Selecione a placa **ESP32-S3** correspondente.
3. Conecte o ESP32‑S3 via USB.
4. Compile e faça o upload.

### Ajustes comuns

- **Calibração da balança:** ajuste `CALIBRATION_FACTOR` em `firmware.ino` caso a massa medida não corresponda ao real.
- **Wi‑Fi/API:** valide `serverBase` e conectividade (o firmware tenta conectar no envio).

## Licença

Este projeto está sob a licença **MIT** — veja `LICENSE`.