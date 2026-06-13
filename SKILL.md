# skill-github-orquestracao

**Versão:** v2.7 — 2026-06-05
**Repositório:** https://github.com/victorarimatea/skl-github-orquestracao
**Mantenedor:** victorarimatea

Esta skill garante a consistência do ecossistema DTD/SETIS a cada operação
realizada nos repositórios GitHub. Toda operação que altera o ecossistema
passa por esta skill — ela lê o estado real antes de agir, planeja todas
as alterações, apresenta o plano para aprovação, e só executa após
autorização explícita do mantenedor.

**Princípio central:** nenhum arquivo do ecossistema fica desatualizado
por esquecimento. O erro não se repete — cada problema encontrado vira
uma verificação adicional nesta skill.

---

## INTENÇÃO DO COMANDANTE

O estado final desejado de toda operação conduzida por esta skill é um
ecossistema internamente consistente, auditável e rastreável — onde o que
existe nos repositórios corresponde ao que está registrado, e o que está
registrado é verdadeiro para o momento em que foi escrito.

Esta declaração é o árbitro de última instância para qualquer situação
não coberta pelas etapas desta skill. Quando o agente encontrar uma
divergência não descrita nos procedimentos, deve perguntar:

**"Esta divergência representa um erro a corrigir, ou um registro
histórico a preservar?"**

A resposta depende da natureza do documento:
- **Texto operacional** (README, INDICE, ONBOARDING, CONTEXTO, SKILL.md)
  descreve o estado *atual* do sistema → divergência é erro, corrigir.
- **Texto histórico** (CHANGELOG, backlog-versoes, backlog-acoes, logs de
  execução) descreve o que *aconteceu* em momento específico → a nomenclatura
  da época é a informação correta, preservar.

A aparência de uma divergência não define sua natureza.
A natureza do documento é que define a ação correta.

---

## IDENTIDADE DO ECOSSISTEMA

- **Usuário GitHub:** victorarimatea
- **Repositório âncora:** hub-fonte
- **Repositório portfólio:** hub-entrada
- **URL base raw:** https://raw.githubusercontent.com/victorarimatea/
- **URL base API:** https://api.github.com/repos/victorarimatea/

---

## QUANDO ESTA SKILL É ACIONADA

Esta skill é acionada sempre que o usuário solicitar qualquer operação
que envolva criação ou modificação de arquivos nos repositórios do
ecossistema. Exemplos:

- "Cria o repositório X"
- "Atualiza o arquivo Y"
- "Registra no ecossistema que..."
- "Adiciona ao backlog..."
- "Quero fazer uma alteração no ecossistema"
- Qualquer operação classificada como OP-A, OP-B, OP-C, OP-D, OP-E ou OP-F
  no protocolo-atualizacoes.md

Esta skill também é acionada **ao encerrar qualquer sessão de trabalho**
que tenha produzido artefatos, decisões ou alterações — mesmo que o
usuário não solicite explicitamente.

---

## ETAPA 0 — Leitura do estado real (obrigatória, sem exceções)

Antes de qualquer outra ação, leia os arquivos abaixo via web_fetch.
Nunca pule esta etapa. O objetivo é conhecer o estado **real e atual**
do ecossistema, não o que foi discutido na sessão.

```
GET https://raw.githubusercontent.com/victorarimatea/hub-fonte/main/sumario.md
GET https://raw.githubusercontent.com/victorarimatea/hub-fonte/main/nomenclatura.md
GET https://raw.githubusercontent.com/victorarimatea/hub-fonte/main/CONTEXTO.md
```

A partir da leitura, extraia e registre internamente:
- Versão atual de cada arquivo que será alterado
- Lista de todos os repositórios ativos (M, S, D, P) com versões
- Próximo ID disponível para cada tipo (próximo Mxx, Sxx, Dxx, Pxx)
- Regras de nomenclatura vigentes

**Se qualquer leitura falhar:** informe o usuário e não prossiga.
A leitura das matrizes é pré-requisito inviolável.

---

## ETAPA 1 — Classificação da operação

Identifique o tipo de operação com base na tabela abaixo.
Uma operação pode combinar mais de um tipo.

| Código | Tipo | Descrição |
|---|---|---|
| OP-A | Criação de repositório | Novo repositório de qualquer tipo (M, S, D, P) |
| OP-B | Atualização de skill | Alteração no SKILL.md ou arquivos de referência de skill existente |
| OP-C | Atualização de matriz | Alteração em arquivo de uma matriz (M01, M02 etc.) |
| OP-D | Geração de documento | Produção de IAC, PoC ou outro instrumento institucional |
| OP-E | Correção pontual | Correção de erro, typo ou inconsistência sem alteração de conteúdo |
| OP-F | Atualização de planejamento | Alteração em ROADMAP, CONTEXTO.md ou documentação de visão |
| OP-P | Atualização de projeto | Qualquer alteração em repositório do tipo P |
| OP-W | Atualização de workflow | Qualquer alteração em repositório do tipo W, incluindo criação de log de execução |
| OP-AG | Atualização de agenda | Depósito de registro de reunião em repositório do tipo A; atualização do INDICE.md cronológico |

Apresente a classificação ao usuário antes de prosseguir:
"Esta operação é classificada como [OP-X + OP-Y]. Vou planejar
as alterações necessárias."

---

## ETAPA 2 — Mapeamento de impacto

Para cada tipo de operação identificado, mapeie **todos** os arquivos
que precisam ser alterados. Use as checklists abaixo como base —
elas são o mínimo obrigatório. Adicione outros arquivos se a
operação específica exigir.

### OP-A — Criação de repositório

**No repositório criado:**
- [ ] `README.md` com ficha técnica (tipo, versão, mantenedor, status)
  - **CONFIRMAR:** após escrita, reler o arquivo e verificar que os campos
    `**Versão:**`, `**Tipo:**` e `**Mantenedor:**` estão presentes e corretos
- [ ] `INDICE.md` com tabela de todos os arquivos criados — **para tipo A:** índice
  cronológico por `data_reuniao` com estrutura de pastas `reunioes/AAAA/MM/`
  (ver Seção 4-C da nomenclatura.md e checklist OP-AG)
- [ ] `INDICE.md` com tabela de todos os arquivos criados (obrigatório em
  todos os repositórios, sem exceção — ver Seção 10 da nomenclatura.md)
- [ ] `backlog-versoes.md` com entrada v1.0 e exposição de motivos
- [ ] `SKILL.md` (apenas tipo S)
  - **CONFIRMAR:** após escrita, reler e verificar que a linha `**Versão:**`
    existe no cabeçalho com o valor correto (v1.0 para nova skill)
- [ ] `stakeholders.md` (apenas tipo P)
- [ ] Pasta `reunioes/` com `.gitkeep` (apenas tipo P)
- [ ] Pasta `documentos/` com `.gitkeep` (apenas tipo P)

**No hub-fonte (M01):**
- [ ] `sumario.md` — nova entrada na seção correta com ID, nome, versão, status, descrição
  - **CONFIRMAR:** após escrita, reler a seção correta do sumario.md e localizar
    a nova entrada — verificar ID, nome e versão; confirmar que não há duplicata
- [ ] `CONTEXTO.md` — tabela de repositórios ativos atualizada
- [ ] `backlog-versoes.md` — nova entrada (tipo: Adição)

**No hub-entrada (portfólio):**
- [ ] `README.md` — diagrama ASCII + tabela de repositórios atualizados
- [ ] `CHANGELOG.md` — nova entrada na versão atual
- [ ] `ROADMAP.md` — item marcado como ✅ se estava planejado

### OP-B — Atualização de skill

**No repositório da skill:**
- [ ] `SKILL.md` — versão incrementada no cabeçalho
  - **CONFIRMAR:** após escrita, reler o cabeçalho do SKILL.md e verificar que
    a linha `**Versão:**` contém exatamente a nova versão esperada. Esta
    confirmação é obrigatória — o Erro #013 (SEV2) foi causado pela ausência
    exata desta verificação pós-escrita.
- [ ] `INDICE.md` — atualizado se a operação criou ou removeu arquivo
  - **CONFIRMAR:** após escrita, reler INDICE.md e verificar que a referência
    à versão do SKILL.md foi atualizada
- [ ] `backlog-versoes.md` — nova entrada com impacto (breaking/non-breaking)
  e skills afetadas
  - **CONFIRMAR:** após escrita, reler as primeiras linhas do backlog e
    verificar que a nova entrada está no topo com a versão correta

**No hub-fonte (M01):**
- [ ] `sumario.md` — versão da skill atualizada
  - **CONFIRMAR:** após escrita, reler a linha da skill no sumario.md e
    verificar que a versão bate com o cabeçalho do SKILL.md
- [ ] `CONTEXTO.md` — tabela de repositórios atualizada
- [ ] `backlog-versoes.md` — nova entrada (tipo: Atualização)

**No hub-entrada:**
- [ ] `CHANGELOG.md` — nova entrada

**Verificação adicional:**
- [ ] A atualização introduz padrão que deve ser propagado a outras skills?
  Se sim, listar quais e abrir item no backlog do M01.

### OP-C — Atualização de matriz

**No repositório da matriz:**
- [ ] Arquivo alterado com versão incrementada no cabeçalho
  - **CONFIRMAR:** após escrita, reler o arquivo alterado e verificar que
    a linha `**Versão:**` contém a nova versão e a data correta
- [ ] `INDICE.md` — atualizado se a operação criou ou removeu arquivo na matriz
- [ ] `backlog-versoes.md` — nova entrada com tópico afetado, fonte, proposto por
- [ ] `CONTEXTO.md` — versão do repositório alterado atualizada na tabela de
  repositórios ativos (a versão no CONTEXTO deve bater com o sumario.md)

**No hub-fonte (M01) — se a matriz alterada NÃO for o próprio M01:**
- [ ] `sumario.md` — versão da matriz atualizada
  - **CONFIRMAR:** após escrita, reler a linha da matriz no sumario.md e
    verificar que a versão bate com o cabeçalho do arquivo alterado
- [ ] `backlog-versoes.md` — nova entrada

**No hub-entrada:**
- [ ] `CHANGELOG.md` — nova entrada se a mudança for relevante

### OP-D — Geração de documento institucional

**No hub-fonte (M01):**
- [ ] `CONTEXTO.md` — seção de documentos gerados atualizada com status
- [ ] `backlog-versoes.md` — nova entrada (tipo: Adição)

**No repositório do projeto associado (tipo P), se existir:**
- [ ] `documentos/` — artefato adicionado
- [ ] `INDICE.md` — atualizado para refletir o novo artefato em `documentos/`
- [ ] `backlog-versoes.md` — nova entrada registrando a geração

**No hub-entrada:**
- [ ] `CHANGELOG.md` — nova entrada se for entrega relevante

### OP-E — Correção pontual

**No arquivo corrigido:**
- [ ] Correção aplicada
  - **CONFIRMAR:** após escrita, reler o trecho corrigido e verificar que
    (a) o texto errado não existe mais e (b) o texto correto está presente
    exatamente como planejado
- [ ] Versão incrementada (MINOR) se o erro comprometia legibilidade,
  rastreabilidade ou consistência do ecossistema; não incrementar para
  typos isolados que não afetam o funcionamento

**No repositório onde ocorreu:**
- [ ] `backlog-versoes.md` — entrada obrigatória se a versão foi incrementada
- [ ] `CHANGELOG.md` do `hub-entrada` — entrada se a correção for relevante
  para o histórico público (omitir para typos menores sem impacto funcional)

**Critério objetivo de incremento de versão em OP-E:**
- **Incrementar:** erro em instrução operacional, link quebrado, nome de arquivo
  errado, versão incorreta registrada, padrão violado
- **Não incrementar:** typo em texto descritivo, ajuste de formatação menor,
  correção ortográfica sem impacto no significado

- [ ] `INDICE.md` — atualizado apenas se a correção envolveu criação ou remoção
  de arquivo (raro em OP-E, mas obrigatório quando ocorrer)

### OP-F — Atualização de planejamento

**No arquivo atualizado:**
- [ ] Conteúdo atualizado
- [ ] Data de atualização ajustada no cabeçalho
- [ ] `INDICE.md` — atualizado se a operação criou novo arquivo referenciado
  no ROADMAP, CONTEXTO.md ou outro documento de planejamento

**No hub-fonte (M01) — se o arquivo for CONTEXTO.md:**
- [ ] `backlog-versoes.md` — nova entrada
- [ ] Versão do CONTEXTO.md incrementada

**No hub-entrada:**
- [ ] `CHANGELOG.md` — nova entrada se mudança for substantiva

### OP-P — Atualização de projeto

**No repositório do projeto:**
- [ ] Arquivo alterado com conteúdo atualizado
- [ ] `INDICE.md` — atualizado se novo arquivo ou pasta foi criado
- [ ] `EXECUCOES.md` — linha adicionada se um workflow foi acionado no contexto
  deste projeto (referência ao log no repositório W — não duplicar conteúdo)
- [ ] `backlog-versoes.md` — nova entrada registrando a decisão/alteração
- [ ] `stakeholders.md` — atualizado se houve mudança de participantes
- [ ] `README.md` — versão atualizada no cabeçalho; estrutura de arquivos e link de navegação rápida atualizados se novo documento foi adicionado
  - **CONFIRMAR:** após escrita, reler o cabeçalho do README.md e verificar
    que a linha `**Versão:**` foi incrementada corretamente

**No hub-fonte (M01):**
- [ ] `sumario.md` — status do projeto atualizado se mudou
  - **CONFIRMAR:** após escrita, reler a linha do projeto no sumario.md e
    verificar que o campo de status está correto
- [ ] `backlog-versoes.md` — nova entrada apenas se o status mudou

**No hub-entrada:**
- [ ] `projetos/monitoramento.md` — atualizado se a mudança for relevante
  para visibilidade externa (somente mediante autorização explícita)

---

#### OP-P especial — hub-memoria (P02)

O P02 é o único repositório do ecossistema que documenta o próprio ecossistema.
Toda modificação no P02 pode afetar arquivos externos que o referenciam.
Usar esta checklist **em substituição** à OP-P genérica quando o repositório
sendo alterado for `hub-memoria`.

**Condição de ativação por tipo de modificação:**

| Tipo de modificação no P02 | sumario.md | CONTEXTO.md | backlog M01 | backlog-acoes | README hub-entrada | CHANGELOG | ROADMAP | monitoramento | arquitetura |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Novo documento interno | versão | versão | ✓ | — | — | — | — | — | — |
| Status muda | versão+status | versão | ✓ | ✓ | — | ✓ | ✓ | ✓ | — |
| Produto formal entregue externamente | versão | ✓ | ✓ | ✓ | — | ✓ | — | — | — |
| Descrição pública muda | — | — | — | — | ✓ | — | — | ✓ | — |
| Mudança na arquitetura do tipo P | — | — | — | — | — | — | — | — | ✓ |
| Marco concluído (próximos passos) | — | ✓ | ✓ | — | — | — | ✓ | — | — |

**Arquivos externos — o que cada um registra sobre o P02:**

*No hub-fonte (M01):*
- [ ] `sumario.md` — versão e status do P02 (fonte de verdade de versão)
- [ ] `CONTEXTO.md` — versão do P02 na tabela; próximos passos relacionados
- [ ] `backlog-versoes.md` — entrada quando status mudou ou CONTEXTO.md foi atualizado
- [ ] `backlog-acoes-dtd.md` — entrada quando produto formal for entregue externamente

*No hub-entrada (portfólio público):*
- [ ] `README.md` — tabela de repositórios (atualizar se descrição mudar)
- [ ] `CHANGELOG.md` — entregas externas relevantes (IAC distribuído, PoC aprovada, marco de fase)
- [ ] `ROADMAP.md` — estado geral do ecossistema (atualizar se fase concluída)
- [ ] `projetos/monitoramento.md` — descrição pública curada (atualizar se escopo ou status público mudar)
- [ ] `docs/arquitetura.md` — apenas se a arquitetura do tipo P mudar

**Regra de ouro:** versão no `sumario.md` é sempre a fonte de verdade.
O `CONTEXTO.md` deriva do `sumario.md` — nunca podem divergir.

---

## ETAPA 3 — Construção do plano

Monte o plano de execução completo **antes de solicitar o token**.
O plano deve ser apresentado como uma tabela clara:

```
PLANO DE EXECUÇÃO — [TIPO DA OPERAÇÃO]
Sessão: [data]
Operação: [descrição em uma linha]

| # | Arquivo | Repositório | Ação | Conteúdo da alteração |
|---|---|---|---|---|
| 1 | sumario.md | hub-fonte | ATUALIZAR v0.X → v0.Y | Adicionar entrada P01 na seção Projetos |
| 2 | CONTEXTO.md | hub-fonte | ATUALIZAR v1.X → v1.Y | Tabela de repositórios: adicionar P01 |
| 3 | backlog-versoes.md | hub-fonte | ADICIONAR entrada | v0.Y — Adição — criação do P01 |
| 4 | README.md | hub-entrada | ATUALIZAR | Diagrama e tabela: adicionar P01 |
| 5 | CHANGELOG.md | hub-entrada | ADICIONAR entrada | [1.X] — P01 criado |
| 6 | README.md | prj-telessaude-poc-prisional | CRIAR | Ficha técnica do projeto |
...

Total: X arquivos em Y repositórios.
Token necessário: escopo 'repo' para os repositórios [lista].
```

Apresente o plano ao usuário e aguarde aprovação explícita.
Não solicite o token antes da aprovação.
Não execute nenhuma ação antes do token ser fornecido.

---

## ETAPA 4 — Solicitação do token

Após aprovação do plano, solicite o token com a mensagem padrão:

```
Plano aprovado. Para executar, preciso de um Personal Access Token
do GitHub com escopo 'repo'.

Repositórios que serão alterados: [lista]
Ações: apenas leitura e escrita de arquivos (sem criação/exclusão
de repositórios, exceto se OP-A estiver no plano)

Cole o token quando estiver pronto. Ele será usado apenas nesta
sessão e pode ser revogado logo após a conclusão em:
https://github.com/settings/tokens
```

---

## ETAPA 5 — Execução

Execute as alterações na ordem do plano, uma por uma.
Para cada arquivo:

1. Obter SHA atual via API:
   `GET https://api.github.com/repos/{user}/{repo}/contents/{path}`

2. Preparar conteúdo novo em base64

3. Enviar via PUT com SHA e mensagem de commit descritiva:
   `PUT https://api.github.com/repos/{user}/{repo}/contents/{path}`
   Body: `{ "message": "...", "content": "...", "sha": "..." }`

4. Verificar resposta: se `content` presente → ✅; se erro → parar e
   informar o usuário antes de continuar

**Regras de execução:**
- Sempre verificar SHA antes de atualizar — nunca assumir que sabe o SHA
- Em caso de erro 409 (conflict): reler o arquivo, obter SHA novo, repetir
- Em caso de erro 401 (unauthorized): informar que o token pode ter expirado
- Em caso de erro em qualquer passo: parar, informar, aguardar instrução
- Nunca tentar "adivinhar" um SHA — sempre ler da API


**Padrão obrigatório para chamadas à API GitHub:**

Toda chamada à API do GitHub deve ser feita via **Python urllib** (ou requests),
nunca via `curl` com heredoc ou interpolação de variáveis bash. A razão é que
curl com heredoc falha silenciosamente quando o conteúdo contém caracteres
especiais (acentos, travessões, aspas), gerando erro de JSON inválido na API.

Padrão correto:

```python
import json, base64, urllib.request

def api_put(token, user, repo, path, message, content_str, sha):
    content_b64 = base64.b64encode(content_str.encode('utf-8')).decode('utf-8')
    payload = json.dumps({
        "message": message,
        "content": content_b64,
        "sha": sha
    }).encode('utf-8')
    req = urllib.request.Request(
        f"https://api.github.com/repos/{user}/{repo}/contents/{path}",
        data=payload, method='PUT',
        headers={
            "Authorization": f"token {token}",
            "Content-Type": "application/json"
        }
    )
    try:
        with urllib.request.urlopen(req) as resp:
            d = json.load(resp)
            print(f"  ✅ {path}")
            return d
    except urllib.error.HTTPError as e:
        body = json.load(e)
        print(f"  ❌ {path} — {body.get('message','erro')}")
        return None

def api_get(token, user, repo, path):
    req = urllib.request.Request(
        f"https://api.github.com/repos/{user}/{repo}/contents/{path}",
        headers={"Authorization": f"token {token}"}
    )
    with urllib.request.urlopen(req) as resp:
        d = json.load(resp)
        return d['sha'], base64.b64decode(d['content']).decode('utf-8')
```

Esta abordagem garante serialização JSON correta independentemente do conteúdo,
trata erros HTTP explicitamente e é portável entre ambientes.

**Ordem recomendada de execução:**
1. Repositórios de conteúdo (o que foi criado/alterado)
2. hub-fonte (M01) — matrizes de registro
3. hub-entrada — portfólio público

Esta ordem garante que se algo falhar no meio, o estado interno
do ecossistema já está atualizado antes da vitrine pública.

---

## ETAPA 6 — Verificação pós-execução

Após todas as alterações, execute verificação de consistência:

**Verificação 1 — Versões:**
Reler `sumario.md` e confirmar que as versões registradas batem
com os cabeçalhos dos arquivos alterados.

Ao varrer entradas em arquivos `backlog-versoes.md`, aceitar **ambos**
os padrões de cabeçalho de entrada:
- `## vX.Y` — padrão usado pela maioria dos repositórios
- `### vX.Y` — padrão usado por repositórios de taxonomia (ex: M02)

Nunca concluir "backlog vazio" sem verificar os dois padrões.
Falso positivo de "sem entradas" gerou diagnóstico incorreto em 2026-06-01
(ver Erro #004).

Ao auditar presença de `INDICE.md` nos repositórios, verificar tanto
na raiz quanto via `git/trees` recursivo — o arquivo pode estar presente
mas não aparecer em listagens superficiais.

**Verificação 2 — README do hub-entrada (comparação obrigatória contra sumario.md):**
Reler o `sumario.md` carregado na Etapa 0. Para cada repositório listado
como ativo no `sumario.md`, verificar se ele aparece no diagrama ASCII
e na tabela do `README.md` do `hub-entrada`. Qualquer repositório presente
no `sumario.md` e ausente do `README.md` configura drift obrigatório a
corrigir antes de encerrar a operação — independentemente de ter sido
criado nesta sessão ou em sessão anterior.

Fonte de verdade: `sumario.md` (já lido na Etapa 0 — não reler).
Este arquivo foi o problema identificado em 2026-06-01 (auto-referencial)
e novamente em 2026-06-03 (drift acumulado de sessões anteriores) —
verificação obrigatória a cada operação, sempre contra o sumario.md.

**Verificação 3 — Links:**
Se novos arquivos foram criados e referenciados em outros documentos,
confirmar que os caminhos estão corretos.

Se qualquer verificação falhar: corrigir imediatamente, ainda com
o token ativo, antes de informar conclusão ao usuário.

**Verificação 5 — Consistência cruzada dos arquivos de referência central:**
Após qualquer operação que crie repositório, altere tipo ou versão, verificar
que os 5 arquivos centrais estão sincronizados entre si:

```python
# Checklist de consistência cruzada (executar mentalmente ou via script)
# 1. sumario.md registra o repositório/versão correta?
# 2. CONTEXTO.md tem a mesma versão que o sumario.md para cada ID?
# 3. README.md do hub-entrada lista o repositório na tabela?
# 4. ROADMAP.md reflete o status real (concluído/em curso/próximo)?
# 5. docs/arquitetura.md descreve os tipos e relações atuais?
# 6. ONBOARDING.md do hub-fonte — campos **Repositório:** e **Versão:** corretos?
```

**Quando esta verificação é obrigatória:**
- Após qualquer OP-A (criação de repositório)
- Após qualquer OP-B ou OP-C que mude versão de componente
- Após qualquer OP-W ou OP-AG que mude status de workflow/agenda

**Se encontrar divergência:** corrigir imediatamente antes do relatório final,
ainda com o token ativo. Registrar como item de correção no relatório.

**Nota:** versões no CONTEXTO.md devem sempre bater com o sumario.md.
O sumario.md é a fonte de verdade — CONTEXTO.md é derivado dele.

**Verificação 5-A — Reconciliação obrigatória com o ROADMAP:**
Ao final de toda operação, para cada entregável produzido nesta sessão,
verificar no `ROADMAP.md` do `hub-entrada`:

- **Se o entregável estava previsto no ROADMAP:** marcar como ✅ com a data
  de conclusão. Exemplo: `- ✅ Criar repositório X (concluído 2026-06-04)`
- **Se o entregável NÃO estava previsto no ROADMAP:** incluí-lo na seção
  ✅ Concluído já marcado como concluído, com data. Nunca omitir — toda
  implementação real deve aparecer no ROADMAP, prevista ou não.

**Propósito:** o ROADMAP deve ser uma linha do tempo cronológica fiel de tudo
que foi construído, não apenas do que foi planejado previamente. Itens não
previstos no início são a norma em ecossistemas em evolução — o registro
retroativo é o que garante rastreabilidade histórica completa.

**Quando esta sub-verificação pode ser omitida:**
Apenas em OP-E (correção pontual de typo/formatação) onde nenhum entregável
novo foi criado. Em todos os demais tipos, é obrigatória.


**Verificação 4 — Glossário:**
Varrer todos os arquivos criados ou alterados na operação em busca de
termos técnicos novos que possam não estar definidos no `GLOSSARIO.md`
do repositório âncora (hub-fonte).

**O que constitui um termo candidato ao glossário:**
- Substantivos técnicos específicos do ecossistema (ex: "drift documental",
  "repositório âncora", "porta de entrada")
- Siglas introduzidas ou usadas com novo significado (ex: OP-A, IAC, PoC)
- Conceitos operacionais novos que não existiam antes da operação
- Adjetivos técnicos com significado específico no contexto (ex: "breaking",
  "non-breaking", "retroalimentado")

**Procedimento:**
1. Listar os termos candidatos identificados nos arquivos alterados
2. Comparar com o conteúdo atual do `GLOSSARIO.md`
3. Para cada termo ausente: propor definição e solicitar aprovação do
   mantenedor antes de atualizar o glossário
4. A atualização do `GLOSSARIO.md` é classificada como OP-C e deve
   seguir o rito completo da S04 — backlog, sumario, INDICE, changelog

**Quando esta verificação pode ser omitida:**
Apenas em OP-E (correção pontual de typo ou formatação) onde nenhum
conteúdo novo foi introduzido. Em todos os demais tipos, é obrigatória.

**Resultado esperado no relatório (Etapa 7):**
- "Glossário verificado: nenhum termo novo identificado" — ou —
- "Glossário verificado: [N] termos propostos → [status]"



---

## ETAPA 6-A — Mineração de ideias

Esta etapa é executada **após todas as verificações da Etapa 6** e **antes
do relatório de encerramento**. É obrigatória em toda sessão que resulte em
plano de ação de edição de repositórios ou documentações.

### Por que esta etapa existe

Boas ideias surgem nas interlinhas de sessões técnicas — num comentário
lateral, numa frase exploratória, num problema identificado ao longo do
caminho. Esta etapa garante que nenhuma ideia embrionária se perca nos
textos das conversas. O agente opera como minerador ativo de contexto,
buscando candidatos, apresentando ao mantenedor e registrando na staging
area após confirmação.

### O que é uma ideia candidata

Uma ideia candidata é qualquer manifestação no histórico da conversa que
**não foi tratada como decisão formal** mas que tem potencial estratégico
para o ecossistema. Ela pode ser embrionária, incompleta, ou expressa de
forma casual — o que importa é o potencial, não a forma.

### Critérios de elegibilidade — perguntas orientadoras

Aplique as perguntas abaixo ao histórico da conversa. Um candidato é
elegível se responder afirmativamente a **pelo menos duas**:

| # | Pergunta orientadora | Sinal típico no texto |
|---|---|---|
| 1 | O mantenedor identificou um problema recorrente ou lacuna no ecossistema? | *"ainda não temos..."*, *"falta um..."*, *"o problema é que..."* |
| 2 | O mantenedor expressou hipótese sobre algo que poderia existir? | *"seria interessante se..."*, *"imagino que..."*, *"poderia ser..."* |
| 3 | O mantenedor fez comparação com referência externa? | *"como fazem em..."*, *"seria como um..."*, *"semelhante ao..."* |
| 4 | O mantenedor formulou pergunta estratégica sem respondê-la? | *"e se..."*, *"por que não..."*, *"o que aconteceria se..."* |
| 5 | O mantenedor expressou desejo ou intenção não convertida em ação? | *"quero pensar mais sobre..."*, *"no futuro..."*, *"seria bom ter..."* |
| 6 | O mantenedor nomeou algo que não existe ainda mas faria sentido existir? | Substantivo novo seguido de descrição funcional |
| 7 | O agente identificou implicação estratégica não explorada na conversa? | Conexão entre dois pontos que o mantenedor não conectou explicitamente |

### Procedimento passo a passo

**Passo 1 — Varredura**
Reler o histórico completo da conversa atual. Não apenas as decisões formais
— incluir comentários laterais, frases exploratórias, problemas identificados
ao longo do caminho.

**Passo 2 — Triagem**
Para cada trecho candidato, aplicar as 7 perguntas orientadoras.
Candidatos com 2 ou mais respostas afirmativas são elegíveis.

**Passo 3 — Apresentação ao mantenedor**
Para cada candidato elegível, apresentar no seguinte formato antes de
qualquer registro:

```
💡 Ideia candidata identificada:
"[trecho ou paráfrase do que foi dito]"

Pergunta orientadora ativada: [número] — [texto da pergunta]
Contexto: [em que momento da conversa surgiu]

Gostaria de registrar isso como ideia na staging area?
```

Aguardar confirmação explícita. **Nunca registrar sem aprovação.**

**Passo 4 — Elaboração**
Após confirmação, elaborar a ideia para registro. A elaboração deve:
- Dar nome descritivo (não apenas repetir a frase original)
- Contextualizar o problema que resolve
- Indicar a qual área do ecossistema se relaciona
- Manter linguagem objetiva — sem superestimar nem subestimar o potencial

**Passo 5 — Depósito na staging area**
Registrar na **Seção C** do arquivo `hub-entrada/staging.md` com todos
os campos preenchidos:

```
| [data] | [conversa de origem] | [ideia elaborada] | [pergunta # ativada] | ✅ confirmado | [data] |
```

**Passo 6 — Resultado no relatório**
Incluir na Etapa 7 o campo:
- `"Mineração de ideias: nenhum candidato identificado"` — ou —
- `"Mineração de ideias: [N] candidatos apresentados → [N aprovados / N recusados]"`

### Quando esta etapa pode ser omitida

Apenas em **OP-E** (correção pontual de typo ou formatação) onde o histórico
da conversa não contém conteúdo substantivo além da correção em si.
Em todos os demais tipos de operação, é obrigatória.

### Onde fica o arquivo de destino

`hub-entrada/staging.md` — Seção C (Ideias mineradas).
A Seção C contém instruções detalhadas sobre o ciclo de vida das ideias,
política de limpeza e legenda de status.

---

### Segundo tipo de candidato — Conhecimento consolidado (hub-aprendizagem)

#### Intenção do Comandante

O hub-aprendizagem é a memória intelectual do ecossistema — não documentação
operacional, mas narrativa de aprendizado sedimentado. Um conhecimento
consolidado é diferente de uma ideia embrionária: ele já passou pelo crivo
da prática, foi confrontado com benchmarks, e resultou em uma lição que
muda como o ecossistema opera. O estado final desejado desta etapa é que
nenhum aprendizado estrutural se perca nos textos de conversas — cada
sessão que produz diagnóstico, decisão arquitetural ou mudança estrutural
deve deixar rastro intelectual no hub-aprendizagem.

#### Critérios de elegibilidade — o aprendizado é consolidado se:

| # | Critério | Pergunta de teste |
|---|---|---|
| 1 | Resolveu um problema recorrente sem solução definitiva até então | *"este problema voltará se não registrarmos o aprendizado?"* |
| 2 | Revelou uma causa raiz que não era óbvia antes | *"antes desta sessão, a causa raiz era conhecida?"* |
| 3 | Produziu decisão arquitetural com raciocínio documentável | *"há contexto, alternativas e consequências que merecem registro?"* |
| 4 | Chegou intuitivamente a algo que benchmarks de mercado validam | *"o que fizemos tem nome, padrão ou literatura de referência?"* |
| 5 | Gerou lição que muda como o ecossistema opera daqui para frente | *"operaremos diferente por causa do que aprendemos hoje?"* |

Elegível com **pelo menos dois** critérios ativados.

#### Procedimento

**Passo 1 — Segunda varredura (após mineração de ideias da Seção C)**
Reler o histórico com foco diferente: não "o que poderia existir" mas
"o que aprendemos". Sinais: "descobrimos que...", "a causa raiz era...",
"o que o mercado chama de...", "se tivéssemos sabido antes...".

**Passo 2 — Apresentação ao mantenedor**

```
📚 Aprendizado consolidado identificado:
"[síntese do aprendizado em 1-2 frases]"

Critérios ativados: [números]
Benchmarks relacionados: [se identificados]
Contexto: [em que momento da sessão surgiu]

Gostaria de registrar e redigir este aprendizado para o hub-aprendizagem?
```

**Passo 3 — Redação imediata após aprovação**
Redigir o aprendizado completo **na mesma sessão**, com contexto fresco.
Formato: narrativa com contexto, diagnóstico, benchmark relacionado e
lição permanente. Não deixar para depois — o contexto da sessão é o
recurso mais valioso e se perde com o tempo.

**Passo 4 — Depósito na Seção E da staging.md**
Status inicial: `pendente`. A inserção formal no hub-aprendizagem
ocorre em sessão dedicada ou quando o mantenedor decidir.

#### Quando esta sub-etapa pode ser omitida

Apenas em **OP-E** (correção pontual) onde o histórico não contém
conteúdo substantivo. Em sessões com diagnóstico, decisão arquitetural
ou mudança estrutural, é obrigatória.

---

## ETAPA 7 — Relatório de encerramento

Apresentar o relatório padrão:

```
ENCERRAMENTO DA OPERAÇÃO
Tipo(s): [OP-X, OP-Y]
Data: [AAAA-MM-DD]
Repositório principal: [nome]

Arquivos atualizados:
  [repo]/[arquivo] → v[X.Y] ✅
  [repo]/[arquivo] → entrada adicionada ✅
  ...

Verificações realizadas:
  Versões consistentes: ✅
  README hub-entrada atualizado: ✅
  Links verificados: ✅
  Glossário verificado: [nenhum termo novo | N termos adicionados]
  Mineração de ideias: [nenhum candidato | N candidatos → N aprovados]

Itens pendentes: [lista ou "nenhum"]

Próxima ação sugerida: [o que o mantenedor deve fazer agora]

Token pode ser revogado: https://github.com/settings/tokens
```

### OP-W — Atualização de workflow

**No repositório do workflow:**
- [ ] `WORKFLOW.md` — atualizado se o processo foi alterado (versão incrementada)
- [ ] `INDICE.md` — atualizado se novo arquivo ou pasta foi criado
- [ ] `backlog-versoes.md` — nova entrada com status do workflow, execuções afetadas,
  skills afetadas

**Log de execução (quando em sessão autenticada com token):**
- [ ] `execucoes/AAAA-MM-DD-[contexto].md` — criado com conteúdo obrigatório:
  data/hora, executor, projeto associado (com link), resumo de etapas,
  decisões tomadas, desvios, artefatos gerados (com links), status, lições
  - **CONFIRMAR:** após criação, verificar via GET que o arquivo existe no
    caminho correto (pasta `execucoes/`, nome com a data real da sessão)

**No projeto associado (tipo P), se houver:**
- [ ] `EXECUCOES.md` — linha adicionada referenciando o log no repositório W
  (não duplicar conteúdo — apenas referenciar)

**No hub-fonte (M01):**
- [ ] `sumario.md` — versão do workflow atualizada se mudou
- [ ] `backlog-versoes.md` — nova entrada se o status mudou

**No hub-entrada:**
- [ ] `CHANGELOG.md` — entrada se a mudança for relevante para o histórico público
- [ ] `ROADMAP.md` — atualizar se o workflow mudou de status (rascunho→ativo,
  ativo→suspenso, etc.) ou se novo workflow foi criado
- [ ] `docs/arquitetura.md` — atualizar se foi criado novo tipo de repositório
  ou se a estrutura de relações entre tipos mudou


### OP-AG — Atualização de agenda

**No repositório da agenda:**
- [ ] Arquivo `AAAA-MM-DD-[CLASSIFICACAO]-[descricao].md` criado na pasta
  `reunioes/AAAA/MM/` correta (baseada em `data_reuniao`, não `data_registro`)
  - **CONFIRMAR:** após criação, verificar via GET que o arquivo está no caminho
    `reunioes/[ano]/[mês]/[nome].md` baseado em `data_reuniao`
- [ ] Front Matter YAML com `data_reuniao` e `data_registro` preenchidos
- [ ] `INDICE.md` — nova linha adicionada na posição cronológica correta
  (ordenado por `data_reuniao`, não por data de inserção)
- [ ] `backlog-versoes.md` — entrada apenas se o repositório A foi alterado
  estruturalmente (nova pasta criada, INDICE.md reestruturado)

**No projeto associado (tipo P), se houver:**
- [ ] `EXECUCOES.md` — linha adicionada referenciando o arquivo no repositório A

**No hub-entrada:**
- [ ] `ROADMAP.md` — atualizar se tipo A ganhou nova funcionalidade relevante

**Atenção: indexação cronológica**
O INDICE.md do tipo A é ordenado por `data_reuniao`. Ao inserir um registro
retroativo (reunião antiga), adicionar na posição correta da linha do tempo —
não necessariamente ao final do arquivo.


---

## ESCALA DE SEVERIDADE DE ERROS

Todo erro registrado nesta skill recebe uma classificação de severidade
baseada no padrão SEV (ITIL / ISO 20000), adaptado ao contexto do ecossistema
DTD/SETIS. A classificação orienta priorização de correções, análise de
tendência e futuras auditorias.

| Nível | Nome | Critério no ecossistema DTD/SETIS |
|---|---|---|
| **SEV1** | Crítico | Instrução operacional incorreta ou ausente — agente executa errado por design |
| **SEV2** | Alto | Dado incorreto em arquivo de referência central (sumario.md, CONTEXTO.md, README) |
| **SEV3** | Médio | Inconsistência interna sem impacto imediato — detectável apenas por auditoria |
| **SEV4** | Baixo | Typo, formatação ou metadado menor sem impacto funcional |

**Regra de uso:** ao registrar um novo erro na seção abaixo, incluir
o campo `**Severidade:**` com o nível SEV correspondente. Erros anteriores
foram classificados retroativamente na versão v2.5 de 2026-06-05.

---

## REGISTRO DE ERROS APRENDIDOS

**Padrão obrigatório de cada entrada** — campos na seguinte ordem:

```
### Erro #NNN — AAAA-MM-DD
**Problema:** [descrição clara do que ocorreu]
**Causa:** [causa raiz identificada]
**Severidade:** [SEV1 | SEV2 | SEV3 | SEV4]
**Correção incorporada:** [o que foi alterado na S04 para evitar recorrência]
**Status:** [Corrigido na vX.Y | Em análise]
```

Entradas mais recentes ficam no topo. Nenhum campo pode ser omitido.
Todo erro deve ter bloco nesta seção E entrada no backlog-versoes.md.

---

### Erro #013 — 2026-06-05
**Problema:** O cabeçalho do SKILL.md ficou travado em v2.1 enquanto o backlog
já registrava v2.4. As operações das versões v2.2, v2.3 e v2.4 atualizaram o
corpo do SKILL.md mas não propagaram o incremento para a linha `**Versão:**`
do cabeçalho. O drift foi detectado na abertura de sessão de 2026-06-05 pela
comparação entre SKILL.md (v2.1), CONTEXTO.md (v2.3) e backlog (v2.4).
**Causa:** A checklist OP-B instrui "versão incrementada no cabeçalho" mas não
tem sub-etapa de confirmação pós-escrita. Instrução sem verificação embutida
é vulnerabilidade estrutural — identificada como Ponto de Falha 1.1 na análise
de engenharia reversa de 2026-06-05.
**Severidade:** SEV2
**Correção incorporada:** Identificado como motivação principal para a Execução 2
(verificações embutidas obrigatórias na S04) da sessão de 2026-06-05.
**Status:** Corrigido na v2.4.1 (cabeçalho) e v2.5 (registro formal)

### Erro #012 — 2026-06-05
**Severidade:** SEV3
**Problema:** O ONBOARDING.md não estava coberto pelas verificações pós-execução
da S04. Após a refatoração de nomenclatura do ecossistema (ecossistema-sumario →
hub-fonte, dtd-setis → hub-entrada), os campos de metadados do ONBOARDING.md,
INDICE.md e README.md permaneceram com o nome legado por múltiplas sessões sem
detecção. Adicionalmente, a seção IDENTIDADE DO ECOSSISTEMA da própria S04
continha os nomes legados dos repositórios âncora e portfólio.
**Causa raiz:** A Verificação 5 monitorava 5 arquivos centrais (sumario.md,
CONTEXTO.md, README.md hub-entrada, ROADMAP.md, arquitetura.md), mas não
incluía o ONBOARDING.md. A S04 também não tinha regra explícita para distinguir
divergências em texto operacional versus texto histórico — gap que poderia gerar
correções indevidas em registros imutáveis de backlog/changelog.
**Solução:** (1) ONBOARDING.md adicionado como item 6 da Verificação 5.
(2) Seção "Intenção do Comandante" adicionada ao topo da skill com a distinção
operacional/histórico como princípio arbitrador para situações não cobertas pelos
procedimentos. (3) IDENTIDADE DO ECOSSISTEMA corrigida para nomenclatura atual
(hub-fonte, hub-entrada).
**Aprendizado geral:** Toda refatoração de nomenclatura deve incluir varredura
explícita em todos os arquivos operacionais do M01 — não apenas nos que fazem
parte das checklists correntes. A Intenção do Comandante serve como salvaguarda
para situações emergentes não antecipadas pelos procedimentos.
**Status:** Corrigido na v2.4

### Erro #011 — 2026-06-04
**Severidade:** SEV4
**Problema:** A S04 não tinha instrução para capturar ideias embrionárias
que surgem nas interlinhas das sessões. Boas ideias expressas de forma casual
ou exploratória durante conversas técnicas se perdiam sem mecanismo de resgate.
**Causa raiz:** A S04 foi concebida como executor de operações — sem papel de
agente de inteligência estratégica. Nenhuma etapa cobria a dimensão de inovação
contínua das sessões de trabalho.
**Solução:** Etapa 6-A adicionada com 7 perguntas orientadoras de elegibilidade,
procedimento de 6 passos (varredura → triagem → apresentação → elaboração →
depósito → relatório) e arquivo de destino staging.md/Seção C. Mineração ocorre
antes do relatório final, ainda com contexto completo disponível.
**Aprendizado geral:** Um agente de IA bem instruído pode operar além da execução
— pode ser minerador ativo de contexto estratégico, desde que com critérios claros
e controle humano na validação. Nenhuma ideia é registrada sem aprovação explícita.
**Status:** Corrigido na v2.3

### Erro #010 — 2026-06-04
**Severidade:** SEV3
**Problema:** O ROADMAP.md acumulava drift silencioso para entregáveis não previstos
originalmente. A S04 só instruía atualizar o ROADMAP quando um item estava previsto
(marcar ✅) — mas não tinha instrução para incluir itens implementados que nunca
foram planejados no ROADMAP. Exemplo concreto: a S07 `skl-briefing-saude-digital`
foi criada, entrou no `sumario.md`, mas nunca apareceu no ROADMAP.
**Causa raiz:** A Verificação 5 checava se o ROADMAP "reflete o status real" de
forma genérica, sem instrução operacional para o caso de entregáveis não previstos.
O W03 (registro de sessão) também não tinha etapa de reconciliação com o ROADMAP.
**Solução:** Verificação 5-A adicionada à Etapa 6: reconciliação obrigatória com
o ROADMAP para todos os entregáveis da sessão — previstos (✅ com data) e não
previstos (incluir já como ✅ com data). W03 recebe Etapa 2-A com a mesma lógica.
**Aprendizado geral:** O ROADMAP deve ser uma linha do tempo cronológica fiel de
tudo que foi construído — não apenas do que foi planejado. Itens emergentes são a
norma em ecossistemas vivos; o registro retroativo é o que garante rastreabilidade.
**Status:** Corrigido na v2.2

### Erro #008 — 2026-06-03
**Problema:** A checklist OP-P não especificava que o README.md do repositório de projeto deve ter versão atualizada no cabeçalho e estrutura de arquivos/link de navegação rápida revisados quando um novo documento é adicionado. Isso gerou a necessidade de identificação manual do drift durante a sessão de 2026-06-03.
**Causa raiz:** A checklist OP-P listava "README.md — status e deliberações pendentes atualizados se necessário", sem mencionar explicitamente a atualização de versão e a revisão da estrutura de arquivos.
**Solução:** Checklist OP-P atualizada: o item do README.md agora especifica versão no cabeçalho + estrutura de arquivos + link de navegação rápida.
**Aprendizado geral:** Ao adicionar qualquer arquivo a um repositório, sempre verificar se o README.md desse repositório precisa refletir a nova estrutura — não apenas status e deliberações.
**Status:** Corrigido na v1.9
**Severidade:** SEV3


Esta seção é atualizada a cada problema encontrado em operações reais.
Cada erro vira uma verificação adicional no fluxo desta skill.

### Erro #009 — 2026-06-03
**Severidade:** SEV2
**Problema:** O `README.md` do `hub-entrada` estava com diagrama ASCII e tabela
de repositórios listando apenas 4 repositórios, enquanto o `sumario.md` registrava
15 repositórios ativos. O drift acumulou silenciosamente ao longo das Fases 2, 3
e 4 do ecossistema (2026-06-01 a 2026-06-02), sem ser detectado pela S04.
**Causa raiz:** A Verificação 2 da Etapa 6 instruía o Claude a confirmar que o
README "reflete o estado atual" — mas sem especificar a fonte de verdade para
essa comparação. O Claude comparava o README com o que a sessão *atual* havia
criado, não com o `sumario.md`. Drift de sessões anteriores era invisível.
**Solução:** Verificação 2 reescrita para exigir comparação explícita do README
contra o `sumario.md` (já lido na Etapa 0), cobrindo tanto a sessão atual quanto
drift acumulado de sessões anteriores. Qualquer ausência é corrigida antes do
encerramento, independentemente de quando o drift foi criado.
**Aprendizado geral:** Verificações de consistência precisam de fonte de verdade
explícita. "Reflete o estado atual" é ambíguo — "bate com o sumario.md" é operacional.
**Status:** Corrigido na v2.1

### Erro #001 — 2026-06-01
**Severidade:** SEV2
**Problema:** README.md do hub-entrada não foi atualizado ao criar o tipo P
e o repositório P01. O diagrama ASCII e a tabela de repositórios ficaram
desatualizados por uma sessão inteira.
**Causa:** A operação OP-A foi executada sem incluir o README do portfólio
no mapeamento de impacto.
**Correção incorporada:** README.md do hub-entrada está agora explícito na
checklist OP-A e é verificado obrigatoriamente na Etapa 6 (Verificação 2).


### Erro #003 — 2026-06-01
**Severidade:** SEV1
**Problema:** Chamadas à API GitHub via `curl` com heredoc bash falharam com
erro "does not match" (HTTP 422) quando o conteúdo dos arquivos continha
caracteres especiais — acentos, travessões, aspas tipográficas. O bash
interpola variáveis e não serializa JSON corretamente com esses caracteres.
**Causa:** Uso de `curl -d "{...}"` com conteúdo gerado por substituição
de variáveis bash. O JSON resultante continha caracteres de controle inválidos.
**Correção incorporada:** Toda chamada à API GitHub usa obrigatoriamente
Python urllib com `json.dumps()` para serialização — nunca curl com heredoc.
Ver padrão de código na Etapa 5 deste SKILL.md.

### Erro #007 — 2026-06-02
**Severidade:** SEV2
**Problema:** Após duas sessões intensas de implementação (criação de tipos W e A,
migração do Cowork, S06, W02, A01), auditoria externa identificou drifts em
CONTEXTO.md (versões M01 e S04 desatualizadas), docs/arquitetura.md (não descrevia
tipos W e A), ROADMAP.md (tratava entregáveis como futuros), dtd-setis/INDICE.md
(contagem e data antigas) e dtd-setis/backlog-versoes.md (parado em v1.0).
**Causa raiz:** A S04 não instruía atualização de CONTEXTO.md em OP-C; não
instruía ROADMAP.md e arquitetura.md em OP-W e OP-AG; não tinha Verificação 5
de consistência cruzada entre os 5 arquivos de referência central. Resultado:
arquivos de referência central acumularam drift sistematicamente.
**Correção incorporada:** OP-C recebe checklist de atualização do CONTEXTO.md;
OP-W e OP-AG recebem ROADMAP.md e arquitetura.md nas checklists; Etapa 6
recebe Verificação 5 de consistência cruzada obrigatória após OP-A, OP-B/C
que mude versão, OP-W e OP-AG.

### Erro #006 — 2026-06-01
**Severidade:** SEV3
**Problema:** Auditoria de cobertura de changelog no OP-P retornou falso
negativo (⚠️) indicando ausência de instrução de changelog. A leitura
manual confirmou que a instrução existe e está correta. O erro é reincidência
do padrão do Erro #005: o script de auditoria buscava 'CHANGELOG.md' em
trecho limitado da seção, não no conteúdo completo capturado por regex.
**Causa:** Uso de busca por substring em variável de texto parcial em vez
de aplicar regex com re.DOTALL na seção completa.
**Correção incorporada:** Toda auditoria de cobertura de checklists da S04
deve usar `re.findall(r'### OP-X.*?(?=### OP-|## )', skill, re.DOTALL)`
para capturar a seção completa antes de verificar presença de termos.
Nunca buscar em trecho — sempre na seção inteira.

### Erro #005 — 2026-06-01
**Severidade:** SEV3
**Problema:** Script de auditoria da Etapa 6 buscava "backlog" e "changelog"
nos primeiros 600 caracteres de cada seção OP-X da S04. As checklists
ficam depois desse limite, fazendo todos os tipos retornarem falso negativo
de "sem instrução de backlog/changelog". A auditoria indicou cobertura zero
quando a cobertura real era de 7/7 para backlog e 6/7 para changelog.
**Causa:** Janela de busca de texto insuficiente — restrita ao cabeçalho
da seção, não ao corpo completo.
**Correção incorporada:** Usar regex com `re.DOTALL` para capturar cada
seção OP-X completa antes de verificar presença de termos. Ver padrão
de auditoria corrigido na Etapa 6.

### Erro #004 — 2026-06-01
**Severidade:** SEV3
**Problema:** Script de auditoria de backlogs usava `'## v'` como padrão
exclusivo de busca de entradas. O repositório `mat-saude-digital-taxonomia` (M02)
usa `'### v'` como cabeçalho de entrada — nível de subseção em vez de seção.
Isso gerou falso positivo de "backlog vazio" no M02, que entrou no diagnóstico
de maturidade como Item 5 ("entrada v1.0 incompleta") e só foi descartado após
leitura manual detalhada do arquivo.
**Causa:** Padrão de busca restrito a um único formato de cabeçalho Markdown,
sem considerar variações válidas entre repositórios.
**Correção incorporada:** A Verificação 1 da Etapa 6 agora instrui
explicitamente a aceitar `## v` e `### v` como padrões válidos de entrada.
Nunca concluir "backlog vazio" sem verificar os dois padrões.

### Erro #002 — histórico anterior a 2026-06-01
**Severidade:** SEV2
**Problema:** Drift de versões — sumário e CONTEXTO.md indicavam versão
v0.5 do M01 enquanto o backlog já registrava v0.9. Ficou assim por
várias sessões sem ser detectado.
**Causa:** Atualizações do backlog-versoes.md não propagavam para o
sumario.md sistematicamente.
**Correção incorporada:** Verificação 1 da Etapa 6 — versões são
comparadas explicitamente após cada operação.

---

## REGRAS DE COMPORTAMENTO

- **Nunca pule a Etapa 0.** As matrizes são a fonte de verdade.
- **Nunca solicite o token antes de apresentar e ter o plano aprovado.**
- **Nunca execute ações parciais.** Se o plano foi aprovado, execute
  tudo. Se algo falhar no meio, informe e aguarde instrução.
- **Nunca armazene o token** além do necessário para a sessão atual.
- **Nunca assuma SHA.** Sempre leia da API antes de atualizar.
- **Sempre verifique o README do dtd-setis** após qualquer OP-A.
- **Se um erro novo for encontrado:** registrar obrigatoriamente em DOIS lugares:
  (1) criar bloco `### Erro #NNN` na seção "REGISTRO DE ERROS APRENDIDOS"
  deste SKILL.md com todos os campos obrigatórios preenchidos; e
  (2) adicionar entrada no `backlog-versoes.md` deste repositório.
  Nunca registrar apenas em um dos dois — o Erro #013 (SEV2) ficou ausente
  da seção de erros por uma sessão inteira por causa exatamente deste gap.
- **Sempre encerre com o relatório da Etapa 7**, mesmo que o usuário
  não solicite.
- **Se encontrar inconsistência não planejada durante a execução:**
  parar, documentar no relatório como item pendente, e propor correção
  antes de considerar a operação concluída.
- **Se um erro novo for encontrado:** registrar em "Registro de Erros
  Aprendidos" neste SKILL.md e propor atualização desta skill ao
  mantenedor.
