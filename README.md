# skill-github-orquestracao

**Tipo:** S — Skill
**ID:** S04
**Versão:** v1.0 — 2026-06-01
**Visibilidade:** Público
**Mantenedor:** victorarimatea

---

## O que é esta skill

A skill de orquestração garante a **consistência do ecossistema DTD/SETIS**
a cada operação realizada nos repositórios GitHub. Toda criação, atualização
ou alteração que afete o ecossistema passa por esta skill.

**Princípio central:** nenhum arquivo fica desatualizado por esquecimento.
O erro não se repete — cada problema encontrado vira uma verificação
permanente incorporada ao fluxo.

---

## Como funciona

A skill opera em duas fases distintas, separadas por uma aprovação explícita:

**Fase 1 — Planejamento (sem token):**
Lê o estado real dos repositórios, classifica a operação, mapeia todos os
arquivos afetados e monta um plano em tabela para aprovação do mantenedor.

**Fase 2 — Execução (com token):**
Somente após aprovação do plano, solicita o token e executa as alterações
na ordem definida, verificando consistência ao final.

---

## Tipos de operação cobertos

| Código | Tipo |
|---|---|
| OP-A | Criação de repositório |
| OP-B | Atualização de skill |
| OP-C | Atualização de matriz |
| OP-D | Geração de documento institucional |
| OP-E | Correção pontual |
| OP-F | Atualização de planejamento |
| OP-P | Atualização de projeto |

---

## Arquivos deste repositório

| Arquivo | Conteúdo |
|---|---|
| `SKILL.md` | Instruções completas para o Claude executar a skill |
| `backlog-versoes.md` | Histórico de versões e alterações |

---

## Contexto institucional

Desenvolvida no âmbito da Diretoria de Transformação Digital (DTD)
da Secretaria Executiva de Tecnologia da Informação em Saúde (SETIS)
da Secretaria de Estado de Saúde do Distrito Federal (SES-DF).

Repositório âncora do ecossistema:
https://github.com/victorarimatea/ecossistema-sumario

---

## Navegação rápida

→ **[INDICE.md](./INDICE.md)** — mapa completo de todos os arquivos deste repositório
