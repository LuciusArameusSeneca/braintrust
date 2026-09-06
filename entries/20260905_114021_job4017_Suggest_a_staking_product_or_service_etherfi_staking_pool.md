# Suggest a staking product or service: ether.fi (staking pool)

**Datum:** 2026-09-05  
**Bewertung:** 85/100  
**Einordnung:** Kein Security-/Audit-Bezug erkannt.

> ⚠️ **Automatisiert gefundener Auftrag, manuell überarbeitet.** Die Kurzbeschreibung stammt aus einer automatisierten, unverifizierten Erfassung und kann ungenau sein. Der ursprüngliche, automatisiert erzeugte "Lösungs"-Code war erfunden bzw. nicht lauffähig und wurde durch ein echtes, getestetes Beispiel ersetzt, das das zugrunde liegende technische Konzept korrekt demonstriert - nicht notwendigerweise eine exakte Lösung für den spezifischen Originalauftrag.

---

## Kurzbeschreibung

Pull-Request an ethereum.org, der ether.fi als Liquid-Staking-Anbieter in ein Verzeichnis von Staking-Produkten einträgt (Logo, Beschreibung, Link). Laut Beschreibung: nicht-custodial Liquid Staking, Node-Operatoren halten die Validator-Schlüssel, Einzahlungen prägen eETH, das zu weETH gewrappt werden kann. Dies ist ein reiner Verzeichnis-/Listing-Eintrag, keine Programmieraufgabe am eigentlichen Protokoll.

## Ergänztes Beispiel (echter, getesteter Code)

Statt des in der automatisierten Rohausgabe erfundenen, gegenstandslosen "Invariant-Checkers" mit nicht existierenden Smart-Contract-Pfaden: eine echte, korrekte Demonstration des tatsächlichen Kernmechanismus hinter Liquid-Staking-Tokens wie eETH/weETH - Nutzer erhalten Anteile (shares) statt eines fixen Token-Betrags, deren ETH-Wert mit eintreffenden Staking-Rewards automatisch steigt:

```python
"""Reale, korrekte Implementierung des Kernmechanismus hinter Liquid-
Staking-Tokens im Stil von eETH/weETH (ether.fi) oder stETH (Lido): Nutzer
zahlen ETH ein und erhalten ANTEILE (shares) statt eines fixen Token-
Betrags. Der Wert eines Anteils steigt mit der Zeit (Staking-Rewards
fliessen in den Pool, ohne dass sich die Anzahl der Anteile aendert) -
das ist das "Rebase"-freie Rechenmodell, das weETH von eETH unterscheidet
(eETH selbst ist rebasing, weETH ist die nicht-rebasende, Exchange-Rate-
basierte Wrapper-Variante fuer DeFi-Kompatibilitaet)."""
from decimal import Decimal


class LiquidStakingPool:
    def __init__(self):
        self.total_pooled_eth = Decimal("0")
        self.total_shares = Decimal("0")
        self.shares_by_user: dict[str, Decimal] = {}

    def _exchange_rate(self) -> Decimal:
        """ETH-Wert pro Anteil. Startet bei 1:1, steigt wenn Rewards
        gutgeschrieben werden, ohne dass sich total_shares aendert."""
        if self.total_shares == 0:
            return Decimal("1")
        return self.total_pooled_eth / self.total_shares

    def deposit(self, user: str, eth_amount: Decimal) -> Decimal:
        """Nutzer zahlt ETH ein, erhaelt Anteile zum AKTUELLEN Wechselkurs
        (nicht 1:1, sobald Rewards den Pool-Wert erhoeht haben)."""
        rate = self._exchange_rate()
        shares_minted = eth_amount / rate
        self.total_pooled_eth += eth_amount
        self.total_shares += shares_minted
        self.shares_by_user[user] = self.shares_by_user.get(user, Decimal("0")) + shares_minted
        return shares_minted

    def accrue_staking_rewards(self, reward_eth: Decimal) -> None:
        """Staking-Rewards erhoehen NUR den Pool-Wert - total_shares bleibt
        unveraendert, wodurch jeder bestehende Anteil automatisch mehr
        ETH wert wird (das ist der ganze Trick des Modells)."""
        self.total_pooled_eth += reward_eth

    def balance_in_eth(self, user: str) -> Decimal:
        shares = self.shares_by_user.get(user, Decimal("0"))
        return shares * self._exchange_rate()


if __name__ == "__main__":
    pool = LiquidStakingPool()

    shares_a = pool.deposit("alice", Decimal("100"))
    print(f"Alice zahlt 100 ETH ein, erhaelt {shares_a:.2f} Anteile (Kurs 1:1 beim Start)")
    assert pool.balance_in_eth("alice") == Decimal("100")

    pool.accrue_staking_rewards(Decimal("10"))  # +10% Rewards auf den Pool
    print(f"Nach Staking-Rewards: Alice's Anteile sind jetzt {pool.balance_in_eth('alice')} ETH wert")
    assert pool.balance_in_eth("alice") == Decimal("110")

    shares_b = pool.deposit("bob", Decimal("110"))
    print(f"Bob zahlt 110 ETH ein (nach den Rewards), erhaelt nur {shares_b:.2f} Anteile (nicht 110!)")
    assert shares_b == Decimal("100")  # Bob bekommt WENIGER Anteile, weil der Kurs gestiegen ist
    assert pool.balance_in_eth("bob") == Decimal("110")

    print("\nAlle Pruefungen bestanden: Anteile spiegeln korrekt den gestiegenen Wechselkurs wider.")
```



---

*Automatisiert erzeugt und archiviert; Code-Beispiel nachträglich manuell ergänzt.*
