# PRD - Bibliotecário Ágora (Livraria Ágora)

## 1. Contexto e Visão do Produto
A Livraria Ágora é um espaço de curadoria focado em desenvolvimento pessoal, filosofia, psicologia, negócios e literatura complexa. O modelo atual de e-commerce falha ao forçar o cliente a buscar por "título" ou "autor". 

O cliente da Ágora frequentemente chega com um dilema existencial, uma transição de carreira ou um momento de luto, buscando um direcionamento que une linguagem e lógica sem cair em clichês de autoajuda. 

O **Bibliotecário Ágora** é um sistema de atendimento autônomo projetado para suprir essa lacuna. Ele atua como um curador empático que compreende a necessidade subjacente e conduz o cliente desde o primeiro desabafo até o fechamento da compra, garantindo precisão de estoque e preço.

## 2. Personas

### A Cliente Contratante: Catarina (Dona da Livraria)
Curadora literária e ex-psicóloga. Ela entende que recomendar o livro errado para alguém em estado de vulnerabilidade pode causar danos. 
**Suas principais dores e medos:**

- **Alucinação Financeira:** Tem pavor que a IA invente promoções, dê descontos irreais ou venda livros fora de estoque.
- **Falta de Controle (O medo da "Mágica"):** Precisa conseguir auditar as conversas para entender por que uma recomendação foi feita.
- **Autonomia sem Supervisão:** Não quer que a IA feche transações irreversíveis sem que alguém da equipe faça uma checagem final de segurança (sanity check).
- **Risco Reputacional:** Medo de que o agente tente atuar como terapeuta em vez de curador, cruzando a linha entre uma sugestão de leitura e um conselho clínico.

### O Usuário Final: Leitor em Busca de Respostas
Profissionais, estudantes e leitores assíduos. Procuram obras profundas e embasadas. Costumam formular pedidos em linguagem natural focados na sua dor (ex: "preciso de algo para lidar com a ansiedade de liderar uma equipe nova").

## 3. Requisitos do Produto (O que queremos construir)

### 3.1. Inteligência e Curadoria (Agentic AI & RAG)
*   O agente deve captar nuances emocionais e filosóficas, traduzindo sentimentos em parâmetros de busca semântica.
*   A recomendação deve ser baseada puramente no catálogo da loja, utilizando metadados enriquecidos (resenhas da Catarina, tags temáticas).
*   A IA deve manter uma postura consultiva, oferecendo insigths através das obras, mas nunca prestando aconselhamento direto.

### 3.2. Blindagem Transacional
*   O agente não tem permissão para calcular preços de cabeça. Toda informação de valor e disponibilidade deve ser extraída em tempo real do banco de dados relacional.
*   O sistema deve impedir a finalização de uma venda sem a validação de estoque no exato momento do checkout.

### 3.3. Fluxo de Aprovação (Human-in-the-Loop)
*   A inteligência conduz a negociação e monta o "carrinho" (Draft Order).
*   Antes da geração do link de pagamento (AbacatePay), o sistema emite um alerta para o dashboard da loja.
*   Catarina ou um membro da equipe revisa o histórico rapidamente e aprova a emissão da cobrança.

### 3.4. Rastreabilidade e Custos
*   Cada interação deve ser monitorada para que seja possível rastrear o fluxo de pensamento da IA (integração com Logfire).
*   O custo por token/sessão deve ser visível para garantir a viabilidade financeira do sistema.

## 4. Jornada do Cliente (Caminho Feliz)
1.  **Descoberta:** Cliente entra no chat e descreve seu momento de vida.
2.  **Curadoria:** Agente investiga (se necessário) e propõe 1 a 2 livros, explicando o *porquê* da recomendação baseado na dor relatada.
3.  **Decisão:** Cliente concorda com a sugestão e pede para comprar.
4.  **Rascunho:** Agente confirma os dados, informa o preço real (consultado no DB) e cria o pedido com status `PENDENTE`.
5.  **Aprovação:** Catarina aprova no dashboard.
6.  **Pagamento:** Cliente recebe o link, paga, e o sistema emite o recibo.

## 5. Escopo (V1)

**O que ENTRA na primeira versão:**

- Interface de chat fluida para o cliente (React).
- Agente principal orquestrado (PydanticAI) com busca híbrida para recomendação (Qdrant).
- Dashboard simples para aprovação de pedidos.
- Integração com gateway de pagamento simulado (AbacatePay).
- Banco de dados relacional *fake* gerado via script Python para representar o catálogo, preços e estoque.

**O que FICA DE FORA da primeira versão:**

- Cálculo de frete real integrado a transportadoras.
- Fluxo de devolução automatizado.
- Recomendações baseadas no histórico de compras passadas do cliente (foco na dor presente).
- Deploy em produção (foco em rodar via Docker Compose localmente).

## 6. Métricas de Sucesso

- **Taxa de Conversão Assistida:** % de conversas que resultam em um pedido enviado para aprovação.
- **Zero Alucinação de Preço:** 100% de precisão nos preços informados no chat comparados ao banco de dados.
- **Taxa de Rejeição da Catarina:** % de pedidos barrados no dashboard (se for muito alto, a IA está recomendando mal).