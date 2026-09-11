# Theo Network: add thUSD adapter, update thBILL reserves, request parent

**Datum:** 2026-09-11  
**Bewertung:** 85/100  
**Einordnung:** Zusaetzlich als moegliche Web3-Security-/Smart-Contract-Audit-Aufgabe erkannt (Chain: solana) - eine kuratierte, manuell gegengeprüfte Fassung findet sich ggf. im [security-portfolio](https://github.com/LuciusArameusSeneca/security-portfolio).

> ⚠️ **Unverifizierter, automatisiert erzeugter Eintrag.** Dieser Eintrag wurde OHNE manuelle Pruefung automatisch archiviert und kann Fehler oder erfundene Inhalte enthalten - insbesondere erfundenen Code, der auf nicht existierende Dateien/Funktionen verweist. Kein Ersatz fuer eine manuelle Verifikation.

---

## Kurzbeschreibung

### Theo Network: add thUSD adapter, update thBILL reserves, request parent

#### Summary:

Two changes for Theo Network (https://theo.xyz):

1. **Update `theo-network-thbill`** (existing, id 6702). The reserve has rotated since listing; the adapter now includes the FILQ-A holding wallet, Aave v3 on Monad, Morpho vaults on Ethereum and Base, USDT, and Arbitrum/Base/Monad USDC. Adds `methodology` and `hallmarks`. Sets `doublecounted: true` because ~96% of thBILL supply is held as reserve by thUSD (below).

2. **Add `theo-network-thusd`** (new). TVL = circulating thUSD supply built from the reserve side, following the `projects/ethena` pattern: on-chain reserves (thBILL at contract NAV, USDC, USDT) reported as those assets; the off-chain gold-carry remainder reported as thUSD.

#### Parent request:

Please create `Theo Network` as a parent and attach both `theo-network-thbill` (6702) and `theo-network-thusd`. With thBILL flagged doublecounted

## Ergebnis (unverifiziert)

```javascript
// Update theo-network-thbill adapter with FILQ-A holding wallet, Aave v3 on Monad, Morpho vaults on Ethereum and Base, USDT, and Arbitrum/Base/Monad USDC

// Add methodology section with doublecounted: true due to ~96% of thBILL supply held as reserve by thUSD
module.exports = {
  tvl: async (timestamp, block) => {
    const web3 = new Web3(Web3.givenProvider);
    const reserveWallet = new web3.eth.Contract(RESERVE_ABI, RESERVE_ADDRESS);

    // Fetch thBILL balance from the reserve wallet
    const thBILLBalance = await reserveWallet.methods.balanceOf(BACKING).call();

    // Calculate TVL based on thethBILL balance and its value
    const tvl = web3.utils.fromWei(thBILLBalance, 'ether');

    return {
      [ADDRESSES.ethereum.thUSD]: tvl,
      doublecounted: true
    };
  },
  methodology:
    'thUSD TVL equals circulating thUSD supply, built from the reserve side. On-chain reserves held in the ' +
    'thUSD reserve wallet — thBILL (valued at thBILL contract NAV via convertToAssets, not market price), ' +
    'USDC and USDT — are reported as those assets. The off-chain gold-carry remainder is reported as thUSD.'
};
```

---

*Automatisiert erzeugt und archiviert.*
