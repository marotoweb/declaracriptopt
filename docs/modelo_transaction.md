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
| `feeFiatValue` | Number | Valor de mercado em euros da taxa de transação no exato momento da operação. Utilizado para deduzir o encargo de forma direta quando a comissão é paga em criptoativos. | Não |
| `fiatValue` | Number | Valor total em FIAT da transação (para `deposit`/`withdrawal`). | Não |
| `tag` | String | **Chave fixa em inglês** que categoriza a transação (`"buy"`, `"sell"`, `"staking"`, etc.). | Não |
| `notes` | String | Notas adicionais do utilizador. | Não |

[Voltar](../README.md)