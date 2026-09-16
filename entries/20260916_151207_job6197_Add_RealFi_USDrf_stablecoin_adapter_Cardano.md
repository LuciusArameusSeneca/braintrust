# Add RealFi USDrf stablecoin adapter (Cardano)

**Datum:** 2026-09-16  
**Bewertung:** 85/100  
**Einordnung:** Zusaetzlich als moegliche Web3-Security-/Smart-Contract-Audit-Aufgabe erkannt (Chain: evm) - eine kuratierte, manuell gegengeprüfte Fassung findet sich ggf. im [security-portfolio](https://github.com/LuciusArameusSeneca/security-portfolio).

> ⚠️ **Unverifizierter, automatisiert erzeugter Eintrag.** Dieser Eintrag wurde OHNE manuelle Pruefung automatisch archiviert und kann Fehler oder erfundene Inhalte enthalten - insbesondere erfundenen Code, der auf nicht existierende Dateien/Funktionen verweist. Kein Ersatz fuer eine manuelle Verifikation.

---

## Kurzbeschreibung

The Add RealFi USDrf stablecoin adapter for Cardano is a pegged issuance adapter that tracks the circulating supply of real-world assets backed by US dollars on the Cardano blockchain, excluding protocol-owned wallets and fees from yield pots. The adapter does not return an actual circulation figure but calculates it as `minted - unreleased`, derived through Blockfrost data.

## Ergebnis (unverifiziert)

```typescript
import { PeggedIssuanceAdapter, Balances } from "../peggedAsset.type";
import { sumSingleBalance } from "../helper/generalUtil";
import {
  getTotalSupply,
  getTokenBalance,
  getAccountAddresses,
} from "../helper/cardano";

// USDrf, minted on Cardano under the RealFi mint proxy script hash.
const USDRF =
  "7d9e4a0ee1a3f5d5ff8159ea91a83310cf2795ee7a87170c7aea05ae55534472";
const DECIMALS = 1e6;

// The unstaked yield pot: staking fees, yield forfeited on unstake, and the
// unstaked share of positive yield. It is protocol-owned until governance sweeps it,
// so none of it is owed to a holder yet.
const YIELD_POT =
  "addr1v9xdjv4h22pv2tq7vugvmyuw0uruue6hmwa86wy624ygs7gq22hrg";

// RealFi's operator wallet: a fee and funding wallet used by the rebalancer and
// operational tooling. USDrf reaching it is protocol working capital, not a liability to any holder.
const OPERATOR_ACCOUNTS = [
  "stake1uyc35qw7ghg4ema2hthwu5xs56zsen48ygkyktfp54w38ecy3ytdd",
];

export class RealFiUSDrfAdapter implements PeggedIssuanceAdapter {
  async minted(): Promise<Balances> {
    const balances = {} as Balances;
    sumSingleBalance(
      balances,
      "peggedUSD",
      await getTotalSupply(USDRF),
      "issued",
      false
    );
    return balances;
  }

  async unreleased(): Promise<Balances> {
    const balances = {} as Balances;

    // Resolve operator addresses at runtime
    const operatorsAddresses = await Promise.all(
      OPERATOR_ACCOUNTS.map((account) => getAccountAddresses(account))
    );

    // Combine operator addresses and yield pot address
    const owners = [YIELD_POT, ...operatorsAddresses.flat().map((i: any) => i.address)];

    for (const owner of owners) {
      const balance = await getTokenBalance(owner, USDRF);
      sumSingleBalance(balances, "peggedUSD", balance, "unreleased", false);
    }

    return balances;
  }

  async circulating(): Promise<number> {
    const mintedBalances = await this.minted();
    const unreleasedBalances = await this.unreleased();

    return (
      parseFloat(mintedBalances.peggedUSD?.issued || "0") -
      parseFloat(unreleasedBalances.peggedUSD?.unreleased || "0")
    );
  }
}
```

---

*Automatisiert erzeugt und archiviert.*
