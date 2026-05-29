# OBD-II Reader

Dispositivo de leitura de dados OBD-II desenvolvido pela [TechGears](https://techgears.app), clube universitário do [Inteli](https://www.inteli.edu.br). Conecta à porta de diagnóstico de qualquer carro e expõe dados do motor em tempo real num dashboard web acessível pelo celular, sem instalar nenhum aplicativo.

> Documentação completa em **[docs.techgears.app/obd-ii-reader](https://docs.techgears.app/obd-II_reader/)**

## Visão Geral

O OBD-II Reader lê dados do motor (rotação, temperatura, velocidade, erros) via porta de diagnóstico padrão presente em todos os carros desde 1996, processa as informações no microcontrolador e as serve num dashboard web acessível via Wi-Fi local, sem necessidade de aplicativo externo.

## Estrutura do repositório

```
obd-II_reader/
├── src/
│   ├── firmware/              # Código do microcontrolador (ESP32)
│   └── hardware/              # Esquemáticos e diagramas de circuito
├── docs/                      # Site de documentação (Docusaurus)
├── lint/
│   └── check-docs.js          # Validador de padrão de documentação
├── docs-meta.json             # Metadados para o portal docs.techgears.app
└── .github/
    └── workflows/
        ├── deploy-docs.yml    # Build e deploy da documentação no GitHub Pages
        ├── lint-docs.yml      # Verificação de padrão de documentação em PRs
        └── pr-description.yml # Preenchimento automático de descrição de PR
```

## Pré-requisitos

| Ferramenta | Uso | Instalação |
|-----------|-----|-----------|
| [Node.js 18+](https://nodejs.org/) | Rodar o site de documentação localmente | nodejs.org |
| [Git](https://git-scm.com/) | Controle de versão | git-scm.com |

## Rodando a documentação localmente

```bash
cd docs
npm install
npm run start
```

O site abre em `http://localhost:3000/obd-II_reader/`.

## Pipelines de CI

| Workflow | Gatilho | O que faz |
|---------|---------|-----------|
| `deploy-docs.yml` | Push em `main` com mudanças em `docs/` | Build e deploy da documentação no GitHub Pages |
| `lint-docs.yml` | Pull Request com mudanças em `docs/docs/` | Valida o padrão de documentação via `check-docs.js` |
| `pr-description.yml` | Abertura de PR | Preenche automaticamente a descrição com tasks em andamento e commits da branch |

## Licença

Distribuído sob a licença [MIT](LICENSE).

Copyright © 2026 TechGears Inteli.
