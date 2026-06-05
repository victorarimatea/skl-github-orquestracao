## v2.7 — 2026-06-05

**Tipo de alteração:** Atualização
**Autorizado por:** Victor Leonardo Arimatea Queiroz — Diretor de Transformação Digital
**Impacto:** non-breaking
**Skills afetadas:** nenhuma

**Exposição de motivos:** Expansão da Etapa 6-A com segundo tipo de mineração:
conhecimentos consolidados candidatos ao hub-aprendizagem. A Etapa 6-A existia
apenas para ideias embrionárias (Seção C da staging). Com o hub-aprendizagem
criado, faltava o mecanismo de captura de aprendizados estruturais — diferentes
de ideias por já terem passado pelo crivo da prática. A expansão adiciona
Intenção do Comandante, 5 critérios de elegibilidade, procedimento de 4 passos
com redação imediata obrigatória e depósito na Seção E da staging.

### Alterações realizadas
- `SKILL.md` v2.6 → v2.7: Etapa 6-A expandida com sub-seção de conhecimento
  consolidado — Intenção do Comandante, critérios, procedimento, regra de
  redação imediata

---

## v2.6 — 2026-06-05

**Tipo de alteração:** Atualização estrutural
**Autorizado por:** Victor Leonardo Arimatea Queiroz — Diretor de Transformação Digital
**Impacto:** non-breaking
**Skills afetadas:** nenhuma

**Exposição de motivos:** Execução 2 da sessão de 2026-06-05. Resultado do
exercício de engenharia reversa que identificou 4 gaps estruturais no ecossistema.
O gap principal (GAP 1 — ausência de verificação independente) foi atacado com
a inserção de verificações embutidas obrigatórias nas checklists de todas as
operações OP-X. Nível B Estrutural: além das verificações, reescrita das regras
de registro de erros e padronização dos campos obrigatórios de cada entrada.

### Transformações aplicadas

**Transformação 1 — Verificações embutidas (CONFIRMAR) nas checklists:**
- OP-A: README.md, SKILL.md (tipo S), sumario.md (3 pontos)
- OP-B: SKILL.md cabeçalho, INDICE.md, backlog, sumario.md (4 pontos)
- OP-C: arquivo alterado cabeçalho, sumario.md (2 pontos)
- OP-E: correção aplicada (1 ponto)
- OP-P: README.md, sumario.md status (2 pontos)
- OP-W: log de execução (1 ponto)
- OP-AG: arquivo de reunião caminho (1 ponto)
Total: 14 blocos CONFIRMAR inseridos

**Transformação 2 — Regra de dois registros obrigatórios para erros novos:**
Todo erro novo deve criar bloco na seção REGISTRO DE ERROS e entrada no
backlog-versoes.md. Nunca apenas um dos dois.

**Transformação 3 — Padrão obrigatório de campos de entrada de erro:**
Campos em ordem fixa: Problema, Causa, Severidade, Correção incorporada, Status.
Instrução adicionada no cabeçalho da seção de erros.

### Alterações realizadas
- `SKILL.md` v2.5 → v2.6: 3 transformações conforme acima

---

## v2.5 — 2026-06-05

**Tipo de alteração:** Atualização
**Autorizado por:** Victor Leonardo Arimatea Queiroz — Diretor de Transformação Digital
**Impacto:** non-breaking
**Skills afetadas:** nenhuma

**Exposição de motivos:** O ecossistema não possuía nenhuma escala formal de
severidade para classificação de erros. Todos os erros #001–#013 estavam no mesmo
nível, impossibilitando priorização, análise de tendência e medição de saúde do
ecossistema ao longo do tempo. Adotado o padrão SEV1–SEV4 (baseado em ITIL/ISO 20000),
adaptado ao contexto de governança documental da DTD/SETIS. Todos os erros históricos
classificados retroativamente. Campo **Severidade:** adicionado ao padrão obrigatório
de registro de novos erros.

### Distribuição de severidade (erros #001–#013)
- SEV1 (Crítico): 1 erro — #003
- SEV2 (Alto): 5 erros — #001, #002, #007, #009, #013
- SEV3 (Médio): 6 erros — #004, #005, #006, #008, #010, #012
- SEV4 (Baixo): 1 erro — #011

### Alterações realizadas
- `SKILL.md` v2.4 → v2.5: seção "ESCALA DE SEVERIDADE DE ERROS" adicionada
  antes do Registro de Erros; campo **Severidade:** inserido em todos os 13 erros
  registrados

---

## v2.4.1 — 2026-06-05

**Tipo de alteração:** Correção pontual (OP-E)
**Autorizado por:** Victor Leonardo Arimatea Queiroz — Diretor de Transformação Digital
**Impacto:** non-breaking
**Skills afetadas:** nenhuma

**Exposição de motivos:** Diagnóstico de inconsistência identificado na abertura
de sessão de 2026-06-05. O cabeçalho do SKILL.md estava registrado como v2.1
enquanto o backlog já acumulava v2.4. Investigação confirmou que o SKILL.md
já continha o conteúdo correto de v2.4 (cabeçalho corrigido na operação v2.4
de hoje), mas sumario.md registrava S04 em v2.1 e CONTEXTO.md em v2.3 — ambos
desatualizados. Sincronização dos arquivos de referência realizada. Erro #013
documentado no SKILL.md e no INDICE.md.

### Alterações realizadas
- `INDICE.md` → atualizado: versão do SKILL.md (v1.3→v2.4), data (2026-06-01→2026-06-05),
  tabela de erros expandida de #005 para #013
- `hub-fonte/sumario.md` → S04: v2.1 → v2.4
- `hub-fonte/CONTEXTO.md` → S04: v2.3 → v2.4
- `hub-fonte/backlog-versoes.md` → entrada de sincronização adicionada

---

## v2.4 — 2026-06-05

**Tipo de alteração:** Atualização
**Autorizado por:** Victor Leonardo Arimatea Queiroz — Diretor de Transformação Digital
**Impacto:** non-breaking
**Skills afetadas:** nenhuma
**Exposição de motivos:** Três melhorias simultâneas originadas de diagnóstico de
drift do ONBOARDING.md e da incorporação do princípio de Intenção do Comandante
ao ecossistema. (1) Seção "Intenção do Comandante" adicionada ao topo da skill —
declara o estado final desejado e introduz o princípio arbitrador operacional/histórico
para situações não cobertas pelos procedimentos: texto operacional representa estado
atual e deve ser corrigido; texto histórico representa estado passado e deve ser
preservado. (2) ONBOARDING.md adicionado à Verificação 5 como arquivo monitorado.
(3) IDENTIDADE DO ECOSSISTEMA corrigida para nomenclatura atual. Erro #012 documentado.

### Alterações realizadas
- Seção "Intenção do Comandante" adicionada antes de IDENTIDADE DO ECOSSISTEMA
- IDENTIDADE DO ECOSSISTEMA: `ecossistema-sumario` → `hub-fonte`; `dtd-setis` → `hub-entrada`
- Verificação 5: item 6 adicionado (ONBOARDING.md do hub-fonte — campos Repositório e Versão corretos)
- Erro #012 registrado na seção de erros aprendidos

---

## v2.3 — 2026-06-04

**Tipo de alteração:** Atualização
**Autorizado por:** victorarimatea
**Impacto:** non-breaking
**Skills afetadas:** nenhuma
**Exposição de motivos:** A S04 não tinha instrução para capturar ideias embrionárias
que surgem nas interlinhas das sessões. Boas ideias se perdiam sem mecanismo de resgate.
Corrigido com a Etapa 6-A — mineração ativa de contexto antes do relatório final.

### Alterações realizadas
- Etapa 6-A adicionada entre Etapa 6 e Etapa 7: mineração ativa de ideias com
  7 perguntas orientadoras de elegibilidade e procedimento de 6 passos
  (varredura → triagem → apresentação → elaboração → depósito → relatório)
- Template do relatório (Etapa 7) atualizado: campo "Mineração de ideias" adicionado
- Erro #011 registrado na seção "Registro de Erros Aprendidos"

---

## v2.2 — 2026-06-04

**Tipo de alteração:** Atualização
**Autorizado por:** victorarimatea
**Impacto:** non-breaking
**Skills afetadas:** nenhuma (verificação interna da S04)
**Exposição de motivos:** Drift identificado: o ROADMAP acumulava itens não
previstos que eram implementados mas nunca registrados. A S04 só cobria o
caso "estava previsto → marcar ✅". O caso inverso não existia. Corrigido
com a Verificação 5-A e o Erro #010.

### Alterações realizadas
- Verificação 5-A adicionada à Etapa 6: reconciliação obrigatória com ROADMAP
  para todos os entregáveis da sessão — previstos (✅ com data) e não previstos
  (incluir retroativamente já como ✅ com data)
- Erro #010 registrado na seção "Registro de Erros Aprendidos"


## v1.8 — 2026-06-02

**Tipo de alteração:** Atualização
**Autorizado por:** victorarimatea
**Impacto:** non-breaking
**Exposição de motivos:** Correção da causa raiz dos drifts identificados em
auditoria externa. A S04 não cobria CONTEXTO.md em OP-C, não cobria ROADMAP.md
e arquitetura.md em OP-W e OP-AG, e não tinha verificação de consistência
cruzada entre os 5 arquivos de referência central. Resultado: arquivos
acumularam drift ao longo de sessões intensas de implementação. Blindagem
estrutural completa adicionada nesta versão.

### Alterações realizadas
- `SKILL.md` → v1.8:
  - OP-C: CONTEXTO.md na checklist (versão deve bater com sumario.md)
  - OP-W: ROADMAP.md e arquitetura.md nas checklists
  - OP-AG: ROADMAP.md na checklist
  - Etapa 6: Verificação 5 — consistência cruzada obrigatória entre
    sumario.md × CONTEXTO.md × README.md × ROADMAP.md × arquitetura.md
  - Erro #007: causa raiz dos drifts documentada e corrigida

---

## v2.0 — 2026-06-03

**Tipo de alteração:** Adição estrutural (MAJOR)
**Autorizado por:** victorarimatea
**Exposição de motivos:** O P02 (ecossistema-dtd-setis) é o único repositório do ecossistema que documenta o próprio ecossistema — criando um grafo de dependências reflexivo que a checklist OP-P genérica não cobria adequadamente. Sessão de 2026-06-03 mapeou sistematicamente os 8 arquivos externos (em M01 e dtd-setis) que referenciam o P02, identificou as condições de ativação de cada um e formalizou esse conhecimento como checklist OP-P especial. Mudança classificada como MAJOR porque introduz uma variante de operação incompatível com a lógica anterior de OP-P genérica.
**Impacto:** breaking — operações OP-P sobre o P02 agora seguem checklist própria
**Skills afetadas:** —

### Alterações realizadas
- `SKILL.md` v1.9 → v2.0: checklist `OP-P especial — ecossistema-dtd-setis (P02)` adicionada após OP-P genérica, com matriz de 8 arquivos externos e condições de ativação por tipo de modificação

---



## v1.9 — 2026-06-03

**Tipo de alteração:** Correção / Melhoria
**Autorizado por:** victorarimatea
**Exposição de motivos:** Sessão de 2026-06-03 identificou que a checklist OP-P não especificava explicitamente a verificação do README.md de repositórios de projeto quando novos documentos são adicionados — apenas mencionava "status e deliberações pendentes". O drift foi identificado durante operação de publicação do RESUMO-EXECUTIVO-v2.0 no P02. Corrigido como Erro #008 e aprendizado permanente.
**Impacto:** non-breaking
**Skills afetadas:** —

### Alterações realizadas
- `SKILL.md` v1.8 → v1.9: checklist OP-P expandida (item README.md agora especifica versão + estrutura de arquivos + link de navegação rápida); Erro #008 adicionado ao registro de erros aprendidos

---



## v1.7 — 2026-06-02

**Tipo de alteração:** Atualização
**Autorizado por:** victorarimatea
**Impacto:** non-breaking
**Exposição de motivos:** Implementação do tipo A no ecossistema. A S04 passa
a cobrir operações em repositórios de agenda com o novo tipo OP-AG, incluindo
a lógica específica de indexação cronológica por data_reuniao — distinta da
indexação por criação usada em todos os outros tipos.

### Alterações realizadas
- `SKILL.md` → v1.7: OP-AG com checklist completa (indexação cronológica,
  data_reuniao vs data_registro, posição no INDICE.md); tabela de tipos atualizada

---

## v1.6 — 2026-06-02

**Tipo de alteração:** Atualização
**Autorizado por:** victorarimatea
**Impacto:** non-breaking
**Exposição de motivos:** Implementação do tipo W no ecossistema. A S04 passa
a cobrir operações em repositórios de workflow com o novo tipo OP-W, incluindo
a criação obrigatória de log de execução em sessões autenticadas. A checklist
OP-P recebe o item EXECUCOES.md para garantir que projetos registrem referências
aos workflows acionados em seu contexto.

### Alterações realizadas
- `SKILL.md` → v1.6: OP-W adicionado com checklist completa incluindo log de
  execução; tabela de tipos atualizada; EXECUCOES.md na checklist OP-P

---

## v1.5 — 2026-06-01

**Tipo de alteração:** Atualização
**Autorizado por:** victorarimatea
**Impacto:** non-breaking
**Skills afetadas:** nenhuma
**Exposição de motivos:** Adição da Verificação 4 (auditoria de glossário)
na Etapa 6 da S04. Fecha o ciclo de manutenção do GLOSSARIO.md: toda
operação que introduza termos novos no ecossistema agora tem etapa
obrigatória de verificação e proposta de atualização antes do encerramento.
O glossário deixa de ser um documento estático e passa a crescer
naturalmente com o ecossistema. Campo "Glossário verificado" adicionado
ao relatório padrão da Etapa 7.

### Alterações realizadas
- `SKILL.md` → v1.5:
  - Etapa 6: Verificação 4 adicionada com critérios, procedimento e
    condição de omissão (apenas OP-E sem conteúdo novo)
  - Etapa 7: campo "Glossário verificado" no relatório padrão

---

## v1.4 — 2026-06-01

**Tipo de alteração:** Atualização
**Autorizado por:** victorarimatea
**Impacto:** non-breaking
**Skills afetadas:** nenhuma
**Exposição de motivos:** Quarto ciclo de aprendizado contínuo da sessão.
Cobertura de INDICE.md expandida para OP-B, OP-C, OP-D, OP-E e OP-F com
instrução condicional (atualizar quando a operação criar ou remover arquivo).
Erro #006 incorporado: reincidência do padrão de auditoria com busca em
trecho limitado — corrigido com instrução de uso de regex re.DOTALL completa.

### Alterações realizadas
- `SKILL.md` → v1.4:
  - OP-B, OP-C, OP-D, OP-E, OP-F: INDICE.md com instrução condicional
  - Seção Erros: Erro #006 registrado com causa e correção

---

## v1.3 — 2026-06-01

**Tipo de alteração:** Atualização
**Autorizado por:** victorarimatea
**Impacto:** non-breaking
**Skills afetadas:** nenhuma
**Exposição de motivos:** Terceiro ciclo de aprendizado contínuo da sessão.
OP-E clarificado com critério objetivo de incremento de versão e instrução
de changelog. INDICE.md adicionado como item obrigatório nas checklists
OP-A e OP-P. Erro #005 (janela de busca insuficiente) incorporado.

### Alterações realizadas
- `SKILL.md` → v1.3:
  - OP-E: critério objetivo de incremento + instrução de changelog
  - OP-A: INDICE.md na checklist
  - OP-P: INDICE.md na checklist
  - Etapa 6: instrução de auditoria de INDICE.md
  - Seção Erros: Erro #005 registrado

---

## v1.2 — 2026-06-01

**Tipo de alteração:** Atualização
**Autorizado por:** victorarimatea
**Impacto:** non-breaking
**Skills afetadas:** nenhuma
**Exposição de motivos:** Incorporação do Erro #004, identificado durante
diagnóstico de maturidade do ecossistema. O script de auditoria da Etapa 6
usava padrão de busca restrito ('## v') que não cobria repositórios com
entradas em subseção ('### v'), como o M02. O falso positivo gerou diagnóstico
incorreto que só foi descartado após leitura manual. Segundo ciclo completo
de aprendizado contínuo da skill nesta mesma sessão de criação.

### Alterações realizadas
- `SKILL.md` → v1.2:
  - Etapa 6, Verificação 1: instrução para aceitar '## v' e '### v'
    como padrões válidos de entrada em backlog-versoes.md
  - Seção Erros: Erro #004 registrado com causa e correção incorporada

---

## v1.1 — 2026-06-01

**Tipo de alteração:** Atualização
**Autorizado por:** victorarimatea
**Impacto:** non-breaking
**Skills afetadas:** nenhuma (melhoria interna de padrão de execução)
**Exposição de motivos:** Incorporação do Erro #003 identificado na mesma
sessão de criação da skill: chamadas à API GitHub via curl com heredoc bash
falharam com caracteres especiais no conteúdo. A correção define Python urllib
como padrão obrigatório para todas as chamadas à API, com funções helper
reutilizáveis (api_put e api_get) documentadas diretamente na Etapa 5.
Este é o primeiro ciclo completo de aprendizado contínuo da skill — erro
identificado, corrigido e registrado na mesma sessão, conforme a Regra 2
do CONTEXTO.md do M01.

### Alterações realizadas
- `SKILL.md` → v1.1:
  - Etapa 5: adição de seção "Padrão obrigatório para chamadas à API GitHub"
    com funções Python helper (api_put, api_get) e justificativa
  - Seção "Registro de Erros Aprendidos": adição do Erro #003 com causa,
    problema e correção incorporada

---

# Backlog de Versões — skill-github-orquestracao

---

## v1.0 — 2026-06-01

**Tipo de alteração:** Criação
**Autorizado por:** victorarimatea
**Impacto:** non-breaking (nova skill, sem dependências existentes)
**Skills afetadas:** todas — esta skill orquestra operações que afetam
qualquer repositório do ecossistema

**Exposição de motivos:** Criação da skill de orquestração do ecossistema
DTD/SETIS. A necessidade surgiu da constatação, ao longo de múltiplas
sessões de trabalho, de que operações no ecossistema dependiam da memória
da sessão para garantir que nenhum arquivo fosse esquecido. Dois erros
concretos foram identificados e motivaram a criação:

1. README.md do dtd-setis não atualizado ao criar o tipo P e o P01
   (identificado em 2026-06-01)
2. Drift de versões entre sumario.md e backlog-versoes.md do M01
   (identificado retrospectivamente, ocorreu em múltiplas sessões)

A skill resolve isso com: leitura obrigatória do estado real antes de
agir; plano completo apresentado antes de qualquer execução; aprovação
explícita antes de solicitar token; verificação de consistência pós-
execução; e registro permanente de erros aprendidos que viram
verificações adicionais.

### Arquivos criados nesta versão
- `README.md` — apresentação pública do repositório
- `SKILL.md` — instruções completas v1.0 (380 linhas)
- `backlog-versoes.md` — este arquivo
