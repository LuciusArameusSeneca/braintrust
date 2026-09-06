# R-#163: Referral Program for learn.tg

**Datum:** 2026-09-05  
**Bewertung:** 75/100  
**Einordnung:** Kein Security-/Audit-Bezug erkannt.

> ⚠️ **Automatisiert gefundener Auftrag, manuell überarbeitet.** Die Kurzbeschreibung stammt aus einer automatisierten, unverifizierten Erfassung und kann ungenau sein. Der ursprüngliche, automatisiert erzeugte "Lösungs"-Code war erfunden bzw. nicht lauffähig und wurde durch ein echtes, getestetes Beispiel ersetzt, das das zugrunde liegende technische Konzept korrekt demonstriert - nicht notwendigerweise eine exakte Lösung für den spezifischen Originalauftrag.

---

## Kurzbeschreibung

Referral-Programm für die Lernplattform learn.tg mit drei Belohnungsformen: 10 % des Kurspreises bei einem vermittelten Kurskauf, 10 % des Stipendienwerts bei einem vermittelten Missions-Stipendium, sowie ein fixer 1-USDT-Bonus, wenn ein vermittelter Pastor den "Global Disciples"-Kurs kauft. Zusätzlich ein dediziertes Referral-Menü und eine Finanzierungsregel für die Auszahlungen.

## Ergänztes Beispiel (echter, getesteter Code)

Die automatisierte Rohausgabe definierte dieselbe Funktion (`calculateReferralRewards`) drei Mal mit unterschiedlicher, inkompatibler Bedeutung und brach mitten im Code ab. Das eigentliche, generalisierbare Muster - mehrere Belohnungsarten über eine gemeinsame Regel-Zuordnung sauber zusammenzuführen - lässt sich echt und korrekt (inkl. exakter Dezimalrechnung statt Floats, um Rundungsfehler bei Geldbeträgen zu vermeiden) demonstrieren:

```python
"""Reale, korrekte, generalisierbare Implementierung eines Referral-Reward-
Rechners mit mehreren unterschiedlichen Belohnungsformen (Prozentsatz vom
Kaufpreis, Prozentsatz vom Stipendienwert, fixer Bonus) - genau das
Kernproblem des Originalauftrags (Referral-Programm mit 3 Belohnungsarten)."""
from dataclasses import dataclass
from decimal import Decimal
from typing import Dict, List


@dataclass
class ReferralEvent:
    referrer_id: str
    kind: str          # "course_purchase" | "scholarship" | "pastor_bonus"
    amount: Decimal     # Kaufpreis bzw. Stipendienwert (bei pastor_bonus: 0)


REWARD_RULES = {
    "course_purchase": lambda amount: amount * Decimal("0.10"),
    "scholarship": lambda amount: amount * Decimal("0.10"),
    "pastor_bonus": lambda amount: Decimal("1.00"),  # fixer USDT-Bonus
}


def calculate_referral_rewards(events: List[ReferralEvent]) -> Dict[str, Decimal]:
    """Summiert die Belohnungen pro Referrer ueber alle Events hinweg.
    Nutzt Decimal statt float, um Rundungsfehler bei Geldbetraegen zu
    vermeiden (klassischer, real relevanter Fehler bei Finanzlogik)."""
    totals: Dict[str, Decimal] = {}
    for event in events:
        rule = REWARD_RULES.get(event.kind)
        if rule is None:
            raise ValueError(f"Unbekannte Belohnungsart: {event.kind}")
        reward = rule(event.amount)
        totals[event.referrer_id] = totals.get(event.referrer_id, Decimal("0")) + reward
    return totals


if __name__ == "__main__":
    events = [
        ReferralEvent("user1", "course_purchase", Decimal("100")),
        ReferralEvent("user1", "scholarship", Decimal("50")),
        ReferralEvent("user2", "pastor_bonus", Decimal("0")),
        ReferralEvent("user1", "course_purchase", Decimal("30")),
    ]
    totals = calculate_referral_rewards(events)
    for referrer, total in totals.items():
        print(f"{referrer}: {total} USDT")

    assert totals["user1"] == Decimal("100") * Decimal("0.10") + Decimal("50") * Decimal("0.10") + Decimal("30") * Decimal("0.10")
    assert totals["user2"] == Decimal("1.00")
    print("Alle Pruefungen bestanden.")
```



---

*Automatisiert erzeugt und archiviert; Code-Beispiel nachträglich manuell ergänzt.*
