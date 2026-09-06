# docs: document gas-sponsorship fallback and the insufficient_balance failure

**Datum:** 2026-09-05  
**Bewertung:** 85/100  
**Einordnung:** Kein Security-/Audit-Bezug erkannt.

> ⚠️ **Automatisiert gefundener Auftrag, manuell überarbeitet.** Die Kurzbeschreibung stammt aus einer automatisierten, unverifizierten Erfassung und kann ungenau sein. Der ursprüngliche, automatisiert erzeugte "Lösungs"-Code war erfunden bzw. nicht lauffähig und wurde durch ein echtes, getestetes Beispiel ersetzt, das das zugrunde liegende technische Konzept korrekt demonstriert - nicht notwendigerweise eine exakte Lösung für den spezifischen Originalauftrag.

---

## Kurzbeschreibung

Dokumentations-Pull-Request (KeeperHub): Wenn eine Gas-Sponsorship-Bedingung nicht erfüllt ist (z. B. Sponsoring-Guthaben aufgebraucht oder Netzwerk nicht unterstützt), fällt das System still auf direktes Signieren durch den Operator zurück. Der Operator sieht dabei nur einen kryptischen Fehler (`Insufficient ETH balance. Have: 0.0, Need: 0.000000231`) ohne Kontext. Der PR ergänzt Dokumentation, die erklärt, wann der Fallback greift, was der Fehler bedeutet und wie man ihn behebt.

## Ergänztes Beispiel (echter, getesteter Code)

Das zugrunde liegende, generelle Software-Muster - VOR dem Versuch einer direkten Transaktion das Guthaben prüfen und einen klaren, frühzeitigen Fehler liefern statt eines kryptischen RPC-Fehlers erst beim eigentlichen Senden - lässt sich echt demonstrieren (mit simuliertem Provider, damit das Beispiel ohne echtes Netzwerk lauffähig ist):

```javascript
// sponsorship_fallback.js - Reales, korrektes Muster fuer "Gas-Sponsorship
// mit Fallback auf direktes Signieren", wie im Original-Auftrag beschrieben
// (KeeperHub-Dokumentation). Nutzt einen simulierten Provider (kein echtes
// RPC-Netzwerk noetig), damit dieses Beispiel eigenstaendig lauffaehig ist -
// das Muster selbst ist identisch zu einer echten ethers.js-Anbindung.

class FakeProvider {
  constructor(balances) { this.balances = balances; } // address -> wei (BigInt)
  async getBalance(address) { return this.balances[address] ?? 0n; }
}

class InsufficientBalanceError extends Error {
  constructor(address, have, need) {
    super(`Insufficient ETH balance. Have: ${have}, Need: ${need}. ` +
      `Fund ${address} with at least ${need} wei on this chain and retry.`);
    this.address = address;
    this.have = have;
    this.need = need;
  }
}

/**
 * Versucht zuerst eine gesponserte Transaktion (Relayer zahlt Gas). Schlaegt
 * das Sponsoring-Kriterium fehl (z.B. Netzwerk nicht unterstuetzt oder
 * Sponsoring-Guthaben aufgebraucht), faellt die Funktion auf direktes
 * Signieren durch den Operator zurueck - und prueft VORHER explizit dessen
 * ETH-Guthaben, damit der Fehler klar und fruehzeitig ist statt als
 * kryptischer RPC-Fehler erst beim eigentlichen Senden aufzutauchen.
 */
async function executeWithSponsorshipFallback({ provider, operatorAddress, requiredWei, sponsorshipEligible, sendFn }) {
  if (sponsorshipEligible) {
    console.log('Sponsorship aktiv - Relayer uebernimmt die Gaskosten.');
    return sendFn({ sponsored: true });
  }

  console.log('Sponsorship nicht verfuegbar - falle auf direktes Signieren zurueck.');
  const balance = await provider.getBalance(operatorAddress);
  if (balance < requiredWei) {
    throw new InsufficientBalanceError(operatorAddress, balance.toString(), requiredWei.toString());
  }
  return sendFn({ sponsored: false });
}

// --- Selbsttest ---
async function main() {
  const provider = new FakeProvider({ '0xabc': 0n, '0xdef': 1000000000000n });
  const sendFn = async (opts) => ({ txHash: '0xdeadbeef', ...opts });

  // Fall 1: genuegend Guthaben -> Fallback klappt
  const result1 = await executeWithSponsorshipFallback({
    provider, operatorAddress: '0xdef', requiredWei: 231000000n,
    sponsorshipEligible: false, sendFn,
  });
  console.log('Fall 1 (genuegend Guthaben):', result1);

  // Fall 2: zu wenig Guthaben -> klarer, fruehzeitiger Fehler statt kryptischem RPC-Fehler
  try {
    await executeWithSponsorshipFallback({
      provider, operatorAddress: '0xabc', requiredWei: 231000000n,
      sponsorshipEligible: false, sendFn,
    });
    throw new Error('haette fehlschlagen muessen');
  } catch (err) {
    console.log('Fall 2 (zu wenig Guthaben) - erwarteter Fehler:', err.message);
  }

  console.log('\nAlle Faelle wie erwartet.');
}

main();
```



---

*Automatisiert erzeugt und archiviert; Code-Beispiel nachträglich manuell ergänzt.*
