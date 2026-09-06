# Amplestocks ($AMPS) Phase 2: vault core, interface reconciliation, integration and invariants

**Datum:** 2026-09-06  
**Bewertung:** 85/100  
**Einordnung:** Zusaetzlich als moegliche Web3-Security-/Smart-Contract-Audit-Aufgabe erkannt (Chain: evm) - eine kuratierte, manuell gegengeprüfte Fassung findet sich ggf. im [security-portfolio](https://github.com/LuciusArameusSeneca/security-portfolio).

> ⚠️ **Automatisiert gefundener Auftrag, manuell überarbeitet.** Die Kurzbeschreibung stammt aus einer automatisierten, unverifizierten Erfassung und kann ungenau sein. Der ursprüngliche, automatisiert erzeugte "Lösungs"-Code war erfunden bzw. nicht lauffähig und wurde durch ein echtes, getestetes Beispiel ersetzt, das das zugrunde liegende technische Konzept korrekt demonstriert - nicht notwendigerweise eine exakte Lösung für den spezifischen Originalauftrag.

---

## Kurzbeschreibung

Fortsetzung des Amplestocks-($AMPS)-Vertrags: `AmpsVault` als unveränderliche Verwahrungs- und NAV-Instanz mit u. a. One-Way-Genesis, NAV-über-Idle-Claims-Berechnung, Markt-Referenzpreis via TWAP, Referenzpreis mit Aufwärtsraten-Limit, NAV-Floor-/Divergenz-Watchdog-Fallbacks, sowie pro-rata-Rücknahme.

## Ergänztes Beispiel (echter, getesteter Code)

Die automatisierte Rohausgabe erfand einen kompletten, nicht-existierenden Mocha/Hardhat-Test gegen eine fiktive `AmpsVault`-Klasse mit frei erfundenen Methoden. Ein Vault mit Anteile-basierter NAV-Buchhaltung wie hier beschrieben ist real anfällig für den bekannten ERC4626-"Inflation-/Donation-Angriff": ein Angreifer zahlt zuerst 1 Wei ein, "spendet" dann direkt (ohne deposit()) einen großen Betrag an den Vertrag und verwässert so den Wechselkurs so stark, dass das nächste Opfer 0 Anteile für seine Einzahlung erhält. Die reale Gegenmaßnahme (virtuelle Anteile/Assets, in OpenZeppelins ERC4626 als `_decimalsOffset()` bekannt) lässt sich korrekt demonstrieren - inklusive Beweis, dass der Angriff OHNE die Gegenmaßnahme tatsächlich funktioniert und MIT ihr fehlschlägt:

```python
"""Reale, korrekte Demonstration des bekannten ERC4626-"Inflation-/Donation-
Angriffs" auf Share-basierte Vault-Vertraege (relevant fuer den Original-
Auftrag: ein Vault-Vertrag mit NAV-pro-Anteil-Buchhaltung) - und wie
"virtuelle Anteile/Assets" (das OpenZeppelin-Gegenmittel, in ERC4626 als
_decimalsOffset() bekannt) den Angriff korrekt verhindern.

DER ANGRIFF (ohne virtuelle Anteile):
1. Angreifer ist der ALLERERSTE Einzahler, zahlt 1 Wei ein -> erhaelt 1 Anteil.
2. Angreifer "spendet" (transferiert direkt, OHNE deposit()) z.B. 1000 ETH
   an den Vault-Vertrag - das erhoeht total_assets, aber NICHT total_shares.
3. Der Wechselkurs (assets/shares) ist jetzt absurd hoch (1001 ETH / 1 Anteil).
4. Ein normales Opfer zahlt z.B. 500 ETH ein: 500 / 1001 rundet zu 0 Anteilen
   (Integer-Rundung nach unten) - das Opfer bekommt NULL Anteile fuer seine
   500 ETH, die de facto dem Angreifer zufallen (der seinen 1 Anteil spaeter
   gegen den gesamten Pool inkl. der 500 ETH des Opfers einloest).

DIE VERTEIDIGUNG (virtuelle Anteile/Assets):
Man addiert eine kleine, konstante Anzahl "virtueller" Anteile/Assets zur
Berechnung hinzu (z.B. 10**decimalsOffset). Das macht die anfaengliche
Verwaesserung durch eine Spende ums Vielfache teurer fuer den Angreifer,
als es dem Opfer schadet - der Angriff wird wirtschaftlich unattraktiv."""


def convert_to_shares_naive(assets: int, total_assets: int, total_shares: int) -> int:
    """OHNE virtuelle Anteile - anfaellig fuer den Inflation-Angriff."""
    if total_shares == 0:
        return assets
    return (assets * total_shares) // total_assets


def convert_to_shares_with_virtual_offset(
    assets: int, total_assets: int, total_shares: int, virtual_offset: int = 10 ** 3,
) -> int:
    """MIT virtuellen Anteilen/Assets (ERC4626-Standardempfehlung) - der
    Nenner/Zaehler wird nie 0 UND eine Spende verwaessert den Wechselkurs
    nur noch marginal statt beliebig stark."""
    return (assets * (total_shares + virtual_offset)) // (total_assets + virtual_offset)


def simulate_attack(convert_fn, virtual_offset: int = 0) -> dict:
    # Schritt 1: Angreifer zahlt 1 Wei ein (erster Einzahler).
    attacker_assets, attacker_shares = 1, 1
    total_assets, total_shares = attacker_assets, attacker_shares

    # Schritt 2: Angreifer "spendet" 1000 ETH direkt an den Vertrag
    # (kein deposit()-Aufruf, daher KEINE neuen Anteile fuer den Angreifer).
    donation = 1000 * 10 ** 18
    total_assets += donation

    # Schritt 3: Opfer zahlt 500 ETH ein.
    victim_deposit = 500 * 10 ** 18
    if virtual_offset:
        victim_shares = convert_fn(victim_deposit, total_assets, total_shares, virtual_offset)
    else:
        victim_shares = convert_fn(victim_deposit, total_assets, total_shares)

    return {
        "attacker_shares": attacker_shares,
        "victim_deposit_eth": victim_deposit / 10 ** 18,
        "victim_shares_received": victim_shares,
        "attack_succeeded": victim_shares == 0,
    }


if __name__ == "__main__":
    naive = simulate_attack(convert_to_shares_naive)
    print("OHNE virtuelle Anteile:", naive)
    assert naive["attack_succeeded"] is True, "Der bekannte Angriff sollte hier IMMER noch funktionieren"

    protected = simulate_attack(convert_to_shares_with_virtual_offset, virtual_offset=10 ** 3)
    print("MIT virtuellen Anteilen (offset=1000):", protected)
    assert protected["attack_succeeded"] is False, "Mit virtuellen Anteilen darf das Opfer NICHT leer ausgehen"
    assert protected["victim_shares_received"] > 0

    print("\nAlle Pruefungen bestanden: virtuelle Anteile verhindern den Inflation-Angriff nachweislich.")
```



---

*Automatisiert erzeugt und archiviert; Code-Beispiel nachträglich manuell ergänzt.*
