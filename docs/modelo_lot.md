# Especificação técnica: Modelo de sub-lotes e inventário (`HybridLot`)

Representa a estrutura de dados utilizada pelo motor analítico em tempo de execução para gerir o inventário de criptoativos em conformidade com o Artigo 43.º, n.º 9 do CIRS. Este modelo suporta a agregação de frações provenientes de transferências neutras através de um histórico detalhado de proveniência (`provenance_history`).

| Campo | Tipo | Descrição | Obrigatório |
| :--- | :--- | :--- | :--- |
| `id` | String | Identificador único do lote (ex: UUID). | Sim |
| `entity_id` | String | O identificador do silo analítico (o nome da exchange ou a constante global de self-custody). | Sim |
| `asset` | String | O símbolo do criptoativo (ex: "BTC", "ETH"). | Sim |
| `acquisition_date` | String | Data em que o lote macro consolidado entrou na carteira atual (ISO 8601). | Sim |
| `amount` | Double | A quantidade total acumulada deste ativo no lote (somatório das sub-frações). | Sim |
| `is_aggregated` | Boolean | Flag que indica se este lote resultou da fusão de múltiplas frações em transferências. | Sim |
| `provenance_history` | List\<Object\> | Histórico detalhado que rastreia a linhagem e as sub-frações originais para efeitos de FIFO e auditoria. | Sim |
| `is_security_token` | Boolean | Sinaliza se o ativo é um valor mobiliário (reitera a perda automática de isenção de 365 dias). | Sim |

---

### Regra de mapeamento de entidades

Para cumprir a legislação fiscal portuguesa, o motor não pode criar pilhas FIFO isoladas para cada endereço físico ou chave pública de carteiras privadas. O pipeline deve injetar os lotes na pilha associando-os ao `entity_id` correto com base no `type` da carteira de destino:

1. **Exchanges (Silo por Plataforma):** Se a carteira for `type: 'Exchange'`, o `entity_id` assume diretamente o nome da plataforma de origem.
   * *Exemplo:* Uma compra na Kraken gera um lote com `entity_id: "Kraken"`.
2. **Self-Custody (Silo Global Unificado):** Se a carteira for `type: 'Cold Wallet'` ou `'Hot Wallet'`, o motor ignora o nome individual (Ledger, Metamask, etc.) e força o agrupamento usando a string única: **`"Self-Custody-Global"`**.
3. **Third-Party (Sem Silo):** Se for `type: 'Other'`, o motor desconsidera as operações nativas e nunca gera um `HybridLot`.

---

### Estrutura de objetos em `provenance_history`

Cada item da lista representa a "certidão de nascimento" fiscal de um sub-lote original individualizado:

| Campo | Tipo | Descrição |
| :--- | :--- | :--- |
| `original_acquisition_date` | String | A data real de compra ou receção inicial do ativo (usada para a contagem estrita dos 365 dias de isenção). |
| `amount` | Double | A fração exata associada a esta compra ou custo histórico específico. |
| `cost_per_unit` | Double | O preço unitário do ativo em Euros (`EUR`) no timestamp original de aquisição. |

---

### Comportamento analítico no _pipeline_ (gestão de pilhas)

O motor manipula a lista de lotes (`List<HybridLot>`) ordenada dinamicamente pela data de aquisição dentro de cada Silo através de duas regras operacionais:

#### 1. Fusão neutra (agregação de lotes)
Ao transferir frações de ativos entre carteiras titulares do mesmo utilizador, o evento é classificado como fiscalmente neutro. O motor localiza o Silo de destino através do `entity_id`:
* Se os ativos forem consolidados (ex: transferência de várias exchanges para uma Ledger), o motor cria um único `HybridLot` macro.
* A flag `is_aggregated` é marcada como `true`.
* O `amount` total passa a ser o somatório das partes.
* As sub-frações individuais são injetadas em `provenance_history`, preservando intactas as suas `original_acquisition_date` e `cost_per_unit` originais.

#### 2. Consumo FIFO fracionado (cisão de lotes)
Quando ocorre uma alienação onerosa (venda para Fiat, permuta por outro ativo ou transferência para uma carteira do tipo `'Other'`), o algoritmo localiza a pilha do Silo correspondente e ordena os elementos contidos em `provenance_history` de forma cronológica pela data mais antiga. Se a venda for parcial:
* O motor abate o `amount` do sub-lote interno que está no topo do FIFO.
* Deduz proporcionalmente o `amount` do lote macro principal.
* Se um sub-lote for totalmente consumido, é removido da lista `provenance_history`.
* Todo o histórico de mutações e a linhagem anterior são mantidos intactos em memória para auditorias futuras da Autoridade Tributária (AT).

---

### Exemplo de Payload `HybridLot` (JSON)

Cenário de um lote de Bitcoin consolidado no Silo Global de Self-Custody, resultante de duas compras fracionadas em períodos diferentes:

```json
{
  "id": "LOT-DEST-001",
  "entity_id": "SELF_CUSTODY_GLOBAL",
  "asset": "BTC",
  "acquisition_date": "2024-06-01T00:00:00.000Z",
  "amount": 0.01603501,
  "is_aggregated": true,
  "provenance_history": [
    {
      "original_acquisition_date": "2024-01-15T00:00:00.000Z",
      "amount": 0.00498500,
      "cost_per_unit": 40000.00
    },
    {
      "original_acquisition_date": "2024-02-10T00:00:00.000Z",
      "amount": 0.01105001,
      "cost_per_unit": 45000.00
    }
  ],
  "is_security_token": false
}

```

[Voltar](../README.md)