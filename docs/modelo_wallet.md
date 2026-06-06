# Especificação técnica: Modelo de entidades (`Wallet`)

Representa uma carteira física, conta numa exchange ou custódia de terceiros mapeada pelo sistema.

| Campo | Tipo | Descrição | Obrigatório |
| :--- | :--- | :--- | :--- |
| `id` | String | Um identificador único para a carteira (ex: UUID). | Sim |
| `name` | String | Nome dado pelo utilizador à carteira (ex: "Kraken", "Ledger Nano X"). | Sim |
| `platform` | String | A plataforma ou tipo de hardware (ex: "Kraken", "Ledger"). | Sim |
| `type` | String | O tipo de carteira. Valores estritos: '*Exchange*', '*Cold Wallet*', '*Hot Wallet*', '*Other*' | Sim |
| `creationDate`| String | A data de criação no formato ISO 8601. | Sim |
| `countryCode` | String | Código de duas letras (ISO 3166-1 alfa-2) do país de sede da plataforma ou de residência associada à carteira. | Sim |
| `fiscalEligibility` | String | **[Calculado]** Classificação do país face à lista de jurisdições fiscais. Valores: `'COOPERATING'`, `'NON_COOPERATING'` (paraísos fiscais), `'UNKNOWN'` (bloqueia o cálculo). | Não |

### Comportamento dos tipos face ao motor analítico
* **`exchange`:** Inicializa um silo FIFO independente e isolado para esta entidade específica.
* **`cold_wallet` / `hot_wallet`:** Os seus saldos e históricos são fundidos e consolidados num único inventário global de *Self-Custody*.
* **`other`:** Sinaliza a **exceção de titularidade** (contas de terceiros/familiares). O motor ignora o saldo interno desta carteira na reconstrução do inventário. Qualquer transferência enviada de uma carteira titular para uma carteira `other` sofre mutação analítica imediata, sendo processada como uma **alienação onerosa** (baixa forçada no FIFO de origem).

### Propriedades calculadas pelo motor
* **`fiscalEligibility`:** Determinado de forma automática no início do processamento. O algoritmo cruza o `countryCode` com uma tabela interna de paraísos fiscais (Portaria n.º 150/2004). O estado `'UNKNOWN'` suspende a execução analítica para impedir cálculos errados em territórios não mapeados pela AT. 
* **Regra para Carteiras `'Other'`:** Transações de entrada ou saída nativas destas carteiras são completamente desconsideradas pelo motor e não geram qualquer cálculo fiscal. O único evento processado é a **transferência recebida por elas** (com origem numa carteira titular), onde o cálculo da mais-valia da alienação é apurado com base nas regras e elegibilidade da **carteira de origem**.

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