# skill-github-orquestracao

**Versão:** v1.9 — 2026-06-03
**Repositório:** https://github.com/victorarimatea/skill-github-orquestracao
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

## IDENTIDADE DO ECOSSISTEMA

- **Usuário GitHub:** victorarimatea
- **Repositório âncora:** ecossistema-sumario
- **Repositório portfólio:** dtd-setis
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
GET https://raw.githubusercontent.com/victorarimatea/ecossistema-sumario/main/sumario.md
GET https://raw.githubusercontent.com/victorarimatea/ecossistema-sumario/main/nomenclatura.md
GET https://raw.githubusercontent.com/victorarimatea/ecossistema-sumario/main/CONTEXTO.md
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
- [ ] `INDICE.md` com tabela de todos os arquivos criados — **para tipo A:** índice
  cronológico por `data_reuniao` com estrutura de pastas `reunioes/AAAA/MM/`
  (ver Seção 4-C da nomenclatura.md e checklist OP-AG)
- [ ] `INDICE.md` com tabela de todos os arquivos criados (obrigatório em
  todos os repositórios, sem exceção — ver Seção 10 da nomenclatura.md)
- [ ] `backlog-versoes.md` com entrada v1.0 e exposição de motivos
- [ ] `SKILL.md` (apenas tipo S)
- [ ] `stakeholders.md` (apenas tipo P)
- [ ] Pasta `reunioes/` com `.gitkeep` (apenas tipo P)
- [ ] Pasta `documentos/` com `.gitkeep` (apenas tipo P)

**No ecossistema-sumario (M01):**
- [ ] `sumario.md` — nova entrada na seção correta com ID, nome, versão, status, descrição
- [ ] `CONTEXTO.md` — tabela de repositórios ativos atualizada
- [ ] `backlog-versoes.md` — nova entrada (tipo: Adição)

**No dtd-setis (portfólio):**
- [ ] `README.md` — diagrama ASCII + tabela de repositórios atualizados
- [ ] `CHANGELOG.md` — nova entrada na versão atual
- [ ] `ROADMAP.md` — item marcado como ✅ se estava planejado

### OP-B — Atualização de skill

**No repositório da skill:**
- [ ] `SKILL.md` — versão incrementada no cabeçalho
- [ ] `INDICE.md` — atualizado se a operação criou ou removeu arquivo
- [ ] `backlog-versoes.md` — nova entrada com impacto (breaking/non-breaking)
  e skills afetadas

**No ecossistema-sumario (M01):**
- [ ] `sumario.md` — versão da skill atualizada
- [ ] `CONTEXTO.md` — tabela de repositórios atualizada
- [ ] `backlog-versoes.md` — nova entrada (tipo: Atualização)

**No dtd-setis:**
- [ ] `CHANGELOG.md` — nova entrada

**Verificação adicional:**
- [ ] A atualização introduz padrão que deve ser propagado a outras skills?
  Se sim, listar quais e abrir item no backlog do M01.

### OP-C — Atualização de matriz

**No repositório da matriz:**
- [ ] Arquivo alterado com versão incrementada no cabeçalho
- [ ] `INDICE.md` — atualizado se a operação criou ou removeu arquivo na matriz
- [ ] `backlog-versoes.md` — nova entrada com tópico afetado, fonte, proposto por
- [ ] `CONTEXTO.md` — versão do repositório alterado atualizada na tabela de
  repositórios ativos (a versão no CONTEXTO deve bater com o sumario.md)

**No ecossistema-sumario (M01) — se a matriz alterada NÃO for o próprio M01:**
- [ ] `sumario.md` — versão da matriz atualizada
- [ ] `backlog-versoes.md` — nova entrada

**No dtd-setis:**
- [ ] `CHANGELOG.md` — nova entrada se a mudança for relevante

### OP-D — Geração de documento institucional

**No ecossistema-sumario (M01):**
- [ ] `CONTEXTO.md` — seção de documentos gerados atualizada com status
- [ ] `backlog-versoes.md` — nova entrada (tipo: Adição)

**No repositório do projeto associado (tipo P), se existir:**
- [ ] `documentos/` — artefato adicionado
- [ ] `INDICE.md` — atualizado para refletir o novo artefato em `documentos/`
- [ ] `backlog-versoes.md` — nova entrada registrando a geração

**No dtd-setis:**
- [ ] `CHANGELOG.md` — nova entrada se for entrega relevante

### OP-E — Correção pontual

**No arquivo corrigido:**
- [ ] Correção aplicada
- [ ] Versão incrementada (MINOR) se o erro comprometia legibilidade,
  rastreabilidade ou consistência do ecossistema; não incrementar para
  typos isolados que não afetam o funcionamento

**No repositório onde ocorreu:**
- [ ] `backlog-versoes.md` — entrada obrigatória se a versão foi incrementada
- [ ] `CHANGELOG.md` do `dtd-setis` — entrada se a correção for relevante
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

**No ecossistema-sumario (M01) — se o arquivo for CONTEXTO.md:**
- [ ] `backlog-versoes.md` — nova entrada
- [ ] Versão do CONTEXTO.md incrementada

**No dtd-setis:**
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

**No ecossistema-sumario (M01):**
- [ ] `sumario.md` — status do projeto atualizado se mudou
- [ ] `backlog-versoes.md` — nova entrada apenas se o status mudou

**No dtd-setis:**
- [ ] `projetos/monitoramento.md` — atualizado se a mudança for relevante
  para visibilidade externa (somente mediante autorização explícita)

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
| 1 | sumario.md | ecossistema-sumario | ATUALIZAR v0.X → v0.Y | Adicionar entrada P01 na seção Projetos |
| 2 | CONTEXTO.md | ecossistema-sumario | ATUALIZAR v1.X → v1.Y | Tabela de repositórios: adicionar P01 |
| 3 | backlog-versoes.md | ecossistema-sumario | ADICIONAR entrada | v0.Y — Adição — criação do P01 |
| 4 | README.md | dtd-setis | ATUALIZAR | Diagrama e tabela: adicionar P01 |
| 5 | CHANGELOG.md | dtd-setis | ADICIONAR entrada | [1.X] — P01 criado |
| 6 | README.md | telessaude-poc-prisional | CRIAR | Ficha técnica do projeto |
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
2. ecossistema-sumario (M01) — matrizes de registro
3. dtd-setis — portfólio público

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

**Verificação 2 — README do dtd-setis:**
Confirmar que o diagrama ASCII e a tabela de repositórios refletem
o estado atual. Este arquivo foi o problema identificado em
2026-06-01 — verificação obrigatória a cada operação.

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
# 3. README.md do dtd-setis lista o repositório na tabela?
# 4. ROADMAP.md reflete o status real (concluído/em curso/próximo)?
# 5. docs/arquitetura.md descreve os tipos e relações atuais?
```

**Quando esta verificação é obrigatória:**
- Após qualquer OP-A (criação de repositório)
- Após qualquer OP-B ou OP-C que mude versão de componente
- Após qualquer OP-W ou OP-AG que mude status de workflow/agenda

**Se encontrar divergência:** corrigir imediatamente antes do relatório final,
ainda com o token ativo. Registrar como item de correção no relatório.

**Nota:** versões no CONTEXTO.md devem sempre bater com o sumario.md.
O sumario.md é a fonte de verdade — CONTEXTO.md é derivado dele.


**Verificação 4 — Glossário:**
Varrer todos os arquivos criados ou alterados na operação em busca de
termos técnicos novos que possam não estar definidos no `GLOSSARIO.md`
do repositório âncora (ecossistema-sumario).

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
  README dtd-setis atualizado: ✅
  Links verificados: ✅
  Glossário verificado: [nenhum termo novo | N termos adicionados]

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

**No projeto associado (tipo P), se houver:**
- [ ] `EXECUCOES.md` — linha adicionada referenciando o log no repositório W
  (não duplicar conteúdo — apenas referenciar)

**No ecossistema-sumario (M01):**
- [ ] `sumario.md` — versão do workflow atualizada se mudou
- [ ] `backlog-versoes.md` — nova entrada se o status mudou

**No dtd-setis:**
- [ ] `CHANGELOG.md` — entrada se a mudança for relevante para o histórico público
- [ ] `ROADMAP.md` — atualizar se o workflow mudou de status (rascunho→ativo,
  ativo→suspenso, etc.) ou se novo workflow foi criado
- [ ] `docs/arquitetura.md` — atualizar se foi criado novo tipo de repositório
  ou se a estrutura de relações entre tipos mudou


### OP-AG — Atualização de agenda

**No repositório da agenda:**
- [ ] Arquivo `AAAA-MM-DD-[CLASSIFICACAO]-[descricao].md` criado na pasta
  `reunioes/AAAA/MM/` correta (baseada em `data_reuniao`, não `data_registro`)
- [ ] Front Matter YAML com `data_reuniao` e `data_registro` preenchidos
- [ ] `INDICE.md` — nova linha adicionada na posição cronológica correta
  (ordenado por `data_reuniao`, não por data de inserção)
- [ ] `backlog-versoes.md` — entrada apenas se o repositório A foi alterado
  estruturalmente (nova pasta criada, INDICE.md reestruturado)

**No projeto associado (tipo P), se houver:**
- [ ] `EXECUCOES.md` — linha adicionada referenciando o arquivo no repositório A

**No dtd-setis:**
- [ ] `ROADMAP.md` — atualizar se tipo A ganhou nova funcionalidade relevante

**Atenção: indexação cronológica**
O INDICE.md do tipo A é ordenado por `data_reuniao`. Ao inserir um registro
retroativo (reunião antiga), adicionar na posição correta da linha do tempo —
não necessariamente ao final do arquivo.


---

## REGISTRO DE ERROS APRENDIDOS

### Erro #008 — 2026-06-03
**Problema:** A checklist OP-P não especificava que o README.md do repositório de projeto deve ter versão atualizada no cabeçalho e estrutura de arquivos/link de navegação rápida revisados quando um novo documento é adicionado. Isso gerou a necessidade de identificação manual do drift durante a sessão de 2026-06-03.
**Causa raiz:** A checklist OP-P listava "README.md — status e deliberações pendentes atualizados se necessário", sem mencionar explicitamente a atualização de versão e a revisão da estrutura de arquivos.
**Solução:** Checklist OP-P atualizada: o item do README.md agora especifica versão no cabeçalho + estrutura de arquivos + link de navegação rápida.
**Aprendizado geral:** Ao adicionar qualquer arquivo a um repositório, sempre verificar se o README.md desse repositório precisa refletir a nova estrutura — não apenas status e deliberações.
**Status:** Corrigido na v1.9


Esta seção é atualizada a cada problema encontrado em operações reais.
Cada erro vira uma verificação adicional no fluxo desta skill.

### Erro #001 — 2026-06-01
**Problema:** README.md do dtd-setis não foi atualizado ao criar o tipo P
e o repositório P01. O diagrama ASCII e a tabela de repositórios ficaram
desatualizados por uma sessão inteira.
**Causa:** A operação OP-A foi executada sem incluir o README do portfólio
no mapeamento de impacto.
**Correção incorporada:** README.md do dtd-setis está agora explícito na
checklist OP-A e é verificado obrigatoriamente na Etapa 6 (Verificação 2).


### Erro #003 — 2026-06-01
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
**Problema:** Script de auditoria de backlogs usava `'## v'` como padrão
exclusivo de busca de entradas. O repositório `saude-digital-taxonomia` (M02)
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
- **Sempre encerre com o relatório da Etapa 7**, mesmo que o usuário
  não solicite.
- **Se encontrar inconsistência não planejada durante a execução:**
  parar, documentar no relatório como item pendente, e propor correção
  antes de considerar a operação concluída.
- **Se um erro novo for encontrado:** registrar em "Registro de Erros
  Aprendidos" neste SKILL.md e propor atualização desta skill ao
  mantenedor.
