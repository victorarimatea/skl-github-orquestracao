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
