Fiz uma versão **análoga**, com estrutura própria. A principal diferença é que Pergunta e Resposta viram **tipos de Mensagem** (generalização) e que há um caso de uso novo, em que o gestor revisa as perguntas sem resposta. Isso muda um pouco o cenário, então coloquei abaixo o trecho a acrescentar.

## 1. Ajustes no cenário (acrescentar ao texto-base)

- **Gestor de conteúdo** (no lugar de "curador"): entra no painel com senha, cadastra documentos do Unespão, acompanha a situação deles, reprocessa os que falharam e exclui os desatualizados. Se a falha se repetir por três tentativas, o documento fica bloqueado e o motivo da última falha fica guardado.
- **Perguntas sem resposta:** quando o assistente avisa que não encontrou a informação, a pergunta fica marcada, e o gestor pode revisar essa lista para descobrir que documento falta (por exemplo, "faltou a tabela de alergênicos do sanduíche").
- **Cliente:** abre o atendimento sem login e faz perguntas sobre cardápio, montagem de lanche, entrega e promoções. Pode avaliar cada resposta como útil ou não útil e, se quiser, deixar um comentário.
- **Conversa:** reúne as mensagens (perguntas do cliente e respostas do assistente) na ordem em que ocorreram. Cada mensagem tem data e hora. A conversa sem mensagens por 30 minutos é encerrada.
- **Fontes:** cada resposta cita os trechos usados, com a relevância de cada um e a posição em que aparece na resposta. Um mesmo trecho pode ser fonte de várias respostas.

## 2. Casos de uso: 3 atores, 13 casos

**Atores:** Cliente, Gestor de conteúdo e Serviço de IA (externo, fora da fronteira).

| # | Caso de uso | Ator ligado |
|---|---|---|
| 1 | Abrir atendimento | Cliente |
| 2 | Perguntar ao Unespão | Cliente |
| 3 | Avaliar resposta | Cliente |
| 4 | Entrar no painel | Gestor |
| 5 | Cadastrar documento | Gestor |
| 6 | Acompanhar documentos | Gestor |
| 7 | Reprocessar documento | Gestor |
| 8 | Excluir documento | Gestor |
| 9 | Revisar perguntas sem resposta | Gestor |
| 10 | Processar documento | (interno) |
| 11 | Recuperar trechos relevantes | (interno) |
| 12 | Gerar resposta com fontes | Serviço de IA |
| 13 | Avisar que não encontrou | (interno) |

**Relações:**
- Perguntar ao Unespão **«include»** Recuperar trechos relevantes.
- Perguntar ao Unespão **«include»** Gerar resposta com fontes.
- Cadastrar documento **«include»** Processar documento.
- Reprocessar documento **«include»** Processar documento.
- Avisar que não encontrou **«extend»** Perguntar ao Unespão, com a condição [nenhum trecho relevante].

O Gestor liga em todos os casos de 4 a 9. Se quiserem mostrar que o login é obrigatório, podem fazer 5 a 9 incluírem "Entrar no painel", mas isso enche o diagrama de setas, então deixei só a associação simples.

**Atenção ao teste rápido do professor:** "Processar documento", "Recuperar trechos relevantes" e "Avisar que não encontrou" são passos internos. Se o grupo achar que eles não passam no teste (o ator ficaria satisfeito só com esse caso?), pode juntá-los aos casos maiores e usar a vaga para algo como "Consultar histórico da conversa". O limite de 9 a 13 permite as duas opções.

## 3. Classes de análise: 8 classes, 1 de associação

| Classe | Atributos |
|---|---|
| Documento | título, tipo de arquivo, origem, enviado em, situação, motivo da falha, tentativas |
| Trecho | ordem, texto |
| Conversa | início, última atividade, encerrada |
| **Mensagem** (superclasse) | texto, enviada em |
| Pergunta | marcada sem resposta |
| Resposta | encontrou informação |
| Avaliação | útil, comentário, avaliada em |
| **Fonte** (classe de associação) | relevância, posição na resposta |

**Relacionamentos:**
- Documento ◆ 1 — 1..* Trecho: **composição**, "dividido em".
- Conversa ◆ 1 — 1..* Mensagem: **composição**, com {ordenadas}.
- **Generalização:** Pergunta e Resposta herdam de Mensagem (seta de ponta triangular vazada apontando para Mensagem).
- Pergunta 1 — 0..1 Resposta: "respondida por".
- Resposta 1 — 0..1 Avaliação: "recebe".
- Resposta 0..* — 0..* Trecho: "cita", com **Fonte** como classe de associação.

Neste modelo, Pergunta e Resposta ficam dentro da Conversa por herdarem de Mensagem. A ordem das mensagens e o horário, que no cenário valem para as duas, ficam em um lugar só.

## 4. O que muda em relação ao exemplo do professor

- Generalização Mensagem → Pergunta/Resposta, no lugar de duas classes soltas.
- Avaliação com comentário e data.
- Pergunta com a marca "sem resposta", que sustenta o caso de uso 9.
- Casos de uso novos: Entrar no painel e Revisar perguntas sem resposta.
- Nomes próprios do domínio: Cliente, Gestor, Conversa, Fonte.

Posso também montar a lista de documentos de exemplo do Unespão (cardápio, alergênicos, entrega) para vocês usarem nas próximas aulas.
