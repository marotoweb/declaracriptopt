# Especificação técnica: Modelo de entidades (`Wallet`)

Representa uma carteira ou conta numa exchange.

| Campo | Tipo | Descrição | Obrigatório |
| :--- | :--- | :--- | :--- |
| `id` | String | Um identificador único para a carteira (ex: UUID). | Sim |
| `name` | String | Nome dado pelo utilizador à carteira (ex: "Kraken", "Ledger Nano X"). | Sim |
| `platform` | String | A plataforma ou tipo de hardware (ex: "Kraken", "Ledger"). | Sim |
| `type` | String | O tipo de carteira. Ex: '*Exchange*', '*Cold Wallet*', '*Hot Wallet*', '*Other*' | Sim |
| `creationDate`| String | A data de criação no formato ISO 8601. | Sim |
| `countryCode` | String | Código de duas letras (ISO 3166-1 alfa-2) do país de sede da plataforma ou de residência associada à carteira. | Sim |
| `fiscalEligibility` | String | **[Calculado]** Classificação do país face à lista de jurisdições fiscais. Valores: `'COOPERATING'`, `'NON_COOPERATING'` (paraísos fiscais), `'UNKNOWN'` (bloqueia o cálculo). | Não |


### Propriedades calculadas pelo motor
* **`fiscalEligibility`:** Determinado de forma automática no início do processamento. O algoritmo cruza o `countryCode` com uma tabela interna de paraísos fiscais (Portaria n.º 150/2004). O estado `'UNKNOWN'` suspende a execução para impedir cálculos errados em jurisdições não mapeadas.

**Exemplo de `Wallet`:**
```json
{
  "id": "wallet-main-001",
  "name": "Kraken Principal",
  "platform": "Kraken",
  "type": "Exchange",
  "creationDate": "2023-01-01T12:00:00.000Z",
  "countryCode": "IE"
}
```
[Voltar](../README.md)