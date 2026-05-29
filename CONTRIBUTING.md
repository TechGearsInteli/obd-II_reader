# Contribuindo com o OBD-II Reader

## Mensagens de commit

Seguimos o padrão [Conventional Commits](https://www.conventionalcommits.org/pt-br):

```
<tipo>: <descrição curta em minúsculas>
```

| Tipo | Quando usar |
|------|-------------|
| `feat` | Nova funcionalidade |
| `fix` | Correção de bug |
| `docs` | Alterações na documentação |
| `refactor` | Refatoração sem mudar comportamento |
| `ci` | Mudanças em workflows ou scripts de CI |
| `chore` | Tarefas de manutenção (deps, configs) |

Exemplos:

```
feat: add continuous OBD-II data reading loop
fix: correct temperature unit conversion for CLT sensor
docs: add hardware block diagram to architecture page
ci: add lint workflow for docs
```

## Branches

Nomeie branches no formato `tipo/descricao-curta`:

```
feat/obd-driver
fix/serial-timeout
docs/architecture-page
```

## Pull Requests

Abra sempre uma PR para `main`. O workflow preenche automaticamente a descrição com suas tasks em andamento e os commits da branch. Complete os campos manualmente antes de pedir revisão.

## Rodando a documentação localmente

```bash
cd docs
npm install
npm run start
```

O site abre em `http://localhost:3000/obd-ii-reader/`.
