# Abner Machado

Engenheiro de software independente, focado em IA aplicada e open source.
Construo produtos SaaS de ponta a ponta e prefiro ferramentas abertas,
auto-hospedaveis e padroes que nao prendem o projeto a um fornecedor.

Meu objetivo e simples: transformar uma ideia em software que roda em
producao, com codigo limpo, mantido por uma pessoa so e sem lock-in.

## Areas de atuacao

- Produtos SaaS completos: arquitetura, backend, deploy e iteracao continua.
- Sistemas RAG: ingestao, chunking, embeddings e recuperacao sobre bancos vetoriais.
- Serving de LLMs: modelos locais com Ollama e vLLM; APIs via OpenRouter e LiteLLM.
- Agentes e orquestracao: LangChain, LangGraph e Model Context Protocol (MCP).

## Como trabalho

- Open source primeiro. Solucao proprietaria so com vantagem tecnica clara.
- Menor interferencia: a mudanca minima que resolve, sem reescrever o que funciona.
- Decisao guiada por custo, manutencao e risco de dependencia, nao por hype.
- Codigo aberto sempre que possivel, para que outros possam auditar e reusar.

## Projetos

### [allied-code](https://github.com/Abner-Machado/allied-code): o guard que decide por jurisprudencia

*Lista de bloqueio decora comandos. Esta aqui lembra o que ja deu errado.*

Um agente de codigo roda comando na sua maquina. Na hora que voce cansa de ler
cada confirmacao, "sim" vira reflexo, e um dia o reflexo aprova a linha errada.

A resposta comum e uma lista de comandos proibidos. O problema da lista e que
ela nao sabe por que proibiu, entao ela erra dos dois lados: bloqueia
`echo "rm -rf /"`, que so imprime texto, e deixa passar a chamada de API que
apaga a pasta inteira sem usar `rm`.

O allied-code inverte isso. A regra sozinha nunca bloqueia nada: ela define um
piso. Quem levanta o piso e um **incidente que ja aconteceu**, gravado em
markdown, recuperado por similaridade e citado no motivo do bloqueio. Se o guard
te barrar, ele te diz qual precedente usou, e voce pode ir ler e discordar.

#### Como funciona, do comando ate o recibo

**1. A acao chega antes de acontecer**
*O agente ja pediu, o shell ainda nao rodou. E a unica janela que existe.*
O guard entra como hook `PreToolUse` do Claude Code. Ele ve a chamada de
ferramenta antes da execucao: comando de shell, escrita de arquivo ou chamada
MCP.

**2. A linha e cortada antes de ser lida**
*`echo "rm -rf /"` imprime. `rm -rf /` apaga. Sao coisas diferentes.*
Um nucleo opcional em Rust separa a linha em segmentos e classifica so os que
executam. Trecho entre aspas nunca e classificado. Substituicao entra na
remontagem do pipeline, que e o que mantem `curl ... | sh` visivel depois de a
linha ser partida.

**3. A classe define o piso**
*Apagar e mandar pra lixeira nao tem o mesmo desfazer.*
Sai uma classe com severidade: `fs.recursive-delete`, `git.history-rewrite`,
`secret.exposure`, `remote.pipe-to-shell`. Chamada MCP tambem tem as suas -
`mcp.destructive` critico para `delete` e `purge`, alto para `trash`, que da pra
desfazer. Verbo que o guard nao conhece vira `mcp.unknown-verb`: silencio nunca
e resposta.

**4. O precedente levanta o piso**
*Se ja aconteceu nesta maquina, a barra sobe.*
A classe abre uma busca no corpus de incidentes. Se aparece um caso parecido e
grave o bastante, a severidade sobe um degrau e o incidente e citado por nome e
data no motivo.

**5. A decisao vem com o porque**
*Bloqueio sem motivo vira "sim" automatico na terceira vez.*
Sai `allow`, `ask` ou `deny`, com o texto dizendo qual classe disparou e qual
precedente sustentou. Em modo observe nada e bloqueado de verdade: o guard so
registra o que teria feito, para voce medir antes de confiar.

**6. Fica o recibo**
*Sem recibo, nao aconteceu.*
Cada decisao vira uma linha no ledger, com a classe, o precedente, a latencia e
se a busca achou algo ou voltou vazia. Segredo e mascarado antes de escrever.

O corpus nao e documentacao: e memoria operacional. Cada incidente diz o que a
maquina fez, o que quebrou, como foi descoberto, e a regra numa frase - mais o
custo dessa regra, porque toda regra cobra um preco. Sao arquivos markdown com
front matter YAML, entao o mesmo corpus abre no Obsidian.

#### Medido, nao prometido

- Classificacao em 5,5 microssegundos com o nucleo Rust, contra 11,4 em Python
  puro. E 2,07x, e a maior parte do ganho teorico se perde na travessia entre as
  linguagens: o nucleo existe pela correcao, nao pela velocidade.
- Rust e Python concordam em oito de nove comandos de referencia. A unica
  divergencia e o comando entre aspas - que e o defeito sendo corrigido.
- 55 testes Python rodando nos dois backends, 43 testes Rust, clippy limpo.
- O nucleo Rust e opcional. Sem ele o guard roda em Python puro e decide igual.

## Stack

### Orquestracao e agentes

<table>
  <tr>
    <td><img src="assets/langchain.svg" alt="LangChain" height="40"></td>
    <td><img src="assets/langgraph.svg" alt="LangGraph" height="40"></td>
    <td><img src="assets/mcp.svg" alt="Model Context Protocol" height="40"></td>
    <td><img src="assets/ai-agent.svg" alt="AI Agents" height="40"></td>
  </tr>
</table>

### Modelos e serving

<table>
  <tr>
    <td><img src="assets/huggingface.svg" alt="Hugging Face" height="40"></td>
    <td><img src="assets/vllm.svg" alt="vLLM" height="40"></td>
    <td><img src="assets/litellm.svg" alt="LiteLLM" height="40"></td>
    <td><img src="assets/openrouter.svg" alt="OpenRouter" height="40"></td>
    <td><img src="assets/codex.svg" alt="Codex" height="40"></td>
  </tr>
</table>

### RAG e embeddings

<table>
  <tr>
    <td><img src="assets/rag.svg" alt="RAG" height="40"></td>
    <td><img src="assets/embedding.svg" alt="Embeddings" height="40"></td>
  </tr>
</table>

### Bancos vetoriais

<table>
  <tr>
    <td><img src="assets/vector-database.svg" alt="Vector Database" height="40"></td>
    <td><img src="assets/chromadb.svg" alt="ChromaDB" height="40"></td>
    <td><img src="assets/faiss.svg" alt="FAISS" height="40"></td>
    <td><img src="assets/milvus.svg" alt="Milvus" height="40"></td>
    <td><img src="assets/qdrant.svg" alt="Qdrant" height="40"></td>
  </tr>
</table>

## Filosofia

Acredito em software livre e em construir na aberta. Ferramenta que da pra
rodar na sua propria maquina, ler o codigo e adaptar vale mais do que caixa
preta conveniente. Aberto a colaboracao em projetos open source.
