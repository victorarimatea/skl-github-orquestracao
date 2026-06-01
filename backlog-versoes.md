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
