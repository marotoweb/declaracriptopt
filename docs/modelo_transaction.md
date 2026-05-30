# Especificação técnica: Modelo de Transações (`Transaction`)

Este documento descreve as propriedades brutas que o motor de cálculo espera receber ao importar movimentos financeiros.

| Campo | Tipo | Descrição | Obrigatório |
| :--- | :--- | :--- | :--- |
| `id` | String | Identificador único da transação. | Sim |
| `date` | String | Data da transação no formato ISO 8601. | Sim |
| `type` | String | Tipo de transação: `"deposit"`, `"withdrawal"`, `"trade"`. | Sim |
| `fromAssetSymbol` | String | Símbolo do ativo de origem (para `withdrawal` e `trade`). | Não |
| `fromAmount` | Number | Quantidade do ativo de origem. | Não |
| `fromWalletId` | String | ID da carteira de origem. | Não |
| `toAssetSymbol` | String | Símbolo do ativo de destino (para `deposit` e `trade`). | Não |
| `toAmount` | Number | Quantidade do ativo de destino. | Não |
| `toWalletId` | String | ID da carteira de destino. | Não |
| `feeAmount` | Number | Quantidade da taxa paga. | Não |
| `feeAssetSymbol` | String | Símbolo do ativo em que a taxa foi paga (ex: "EUR", "BNB"). | Não |
| `feeFiatValue` | Number | Valor em euros da taxa no exato momento da operação. Deve ser sempre expresso em euros, independentemente do ativo usado para pagar a comissão. Utilizado para deduzir o encargo (Art. 51.º do CIRS) e para dar baixa física da fração no inventário FIFO. **Condicional** (ver regra abaixo). | Condicional |
| `fiatValue` | Number | Valor total em FIAT da transação (para `deposit`/`withdrawal`). | Não |
| `tag` | String | **Chave fixa em inglês** que categoriza a transação (`"buy"`, `"sell"`, `"staking"`, etc.). | Não |
| `notes` | String | Notas adicionais do utilizador. | Não |

### Regra de obrigatoriedade de `feeFiatValue`

`feeFiatValue` é **obrigatório** quando as três condições se verificam em simultâneo:

1. `feeAmount` está preenchido e é maior que zero.
2. `feeAssetSymbol` é um criptoativo (qualquer valor diferente de `"EUR"` ou outra moeda fiat).
3. `type` não é `"deposit"` com `tag` de rendimento passivo (`"staking"`, `"airdrop"`, `"interest"`, `"rewards"`, `"defi"`).

Nos depósitos de rendimento passivo, o protocolo suporta o custo de rede ou este simplesmente não existe do ponto de vista do recetor, pelo que `feeFiatValue` não é aplicável e é ignorado mesmo que preenchido.

Se as três condições se verificarem e `feeFiatValue` estiver ausente ou nulo, o motor marca a transação com o estado `INCOMPLETE`, exclui-a do cálculo de mais-valias e do relatório de exportação, e regista-a na lista de transações pendentes para revisão. O valor pode ser fornecido manualmente pelo utilizador ou obtido por API de cotação histórica.

**Exemplos de `Transaction`:**

*   **Compra (Depósito Fiat):**
    ```json
    {
      "id": "tx_compra_01",
      "date": "2024-01-20T10:30:00Z",
      "type": "deposit",
      "toAssetSymbol": "BTC",
      "toAmount": 0.2,
      "toWalletId": "wallet-main-001",
      "fiatValue": 7000.00,
      "feeAmount": 14.00,
      "feeAssetSymbol": "EUR",
      "tag": "buy"
    }
    ```

*   **Venda (Levantamento Fiat):**
    ```json
    {
      "id": "tx_venda_01",
      "date": "2024-05-22T09:00:00Z",
      "type": "withdrawal",
      "fromAssetSymbol": "BTC",
      "fromAmount": 0.1,
      "fromWalletId": "wallet-main-001",
      "fiatValue": 4500.00,
      "feeAmount": 9.00,
      "feeAssetSymbol": "EUR",
      "tag": "sell"
    }
    ```

*   **Recompensa de Staking (sem `feeFiatValue`):**
    ```json
    {
      "id": "tx_stake_01",
      "date": "2024-09-25T13:00:00Z",
      "type": "deposit",
      "toAssetSymbol": "ADA",
      "toAmount": 28.5,
      "toWalletId": "wallet-main-001",
      "tag": "staking",
      "notes": "Recompensa mensal de staking"
    }
    ```

*   **Trade (Cripto para Cripto) com taxa em cripto — `feeFiatValue` obrigatório:**
    ```json
    {
      "id": "tx_trade_01",
      "date": "2024-04-10T14:20:00Z",
      "type": "trade",
      "fromAssetSymbol": "BTC",
      "fromAmount": 0.05,
      "fromWalletId": "wallet-main-001",
      "toAssetSymbol": "ETH",
      "toAmount": 1.0,
      "toWalletId": "wallet-main-001",
      "feeAmount": 0.001,
      "feeAssetSymbol": "ETH",
      "feeFiatValue": 2.85,
      "tag": "trade"
    }
    ```

[Voltar](../README.md)
