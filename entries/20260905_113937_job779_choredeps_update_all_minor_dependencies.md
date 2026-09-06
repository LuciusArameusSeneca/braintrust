# chore(deps): update all minor dependencies

**Datum:** 2026-09-05  
**Bewertung:** 72/100  
**Einordnung:** Zusaetzlich als moegliche Web3-Security-/Smart-Contract-Audit-Aufgabe erkannt (Chain: solana) - eine kuratierte, manuell gegengeprüfte Fassung findet sich ggf. im [security-portfolio](https://github.com/LuciusArameusSeneca/security-portfolio).

> ⚠️ **Automatisiert gefundener Auftrag, manuell überarbeitet.** Die Kurzbeschreibung stammt aus einer automatisierten, unverifizierten Erfassung und kann ungenau sein. Der ursprüngliche, automatisiert erzeugte "Lösungs"-Code war erfunden bzw. nicht lauffähig und wurde durch ein echtes, getestetes Beispiel ersetzt, das das zugrunde liegende technische Konzept korrekt demonstriert - nicht notwendigerweise eine exakte Lösung für den spezifischen Originalauftrag.

---

## Kurzbeschreibung

Automatisierter Renovate-Bot-Pull-Request für ein WordPress-Repository: hebt mehrere `@wordpress/*`-npm-Pakete (u. a. `@wordpress/blocks` von 15.24.0 auf 15.26.0) auf ihre jeweils neueste Minor-Version an.

**Hinweis zur Einordnung:** Die automatisierte Vorab-Klassifikation hat diesen Auftrag fälschlich als möglichen Solana-/Smart-Contract-Bezug markiert (False Positive) - es handelt sich um ein reines WordPress/npm-Dependency-Update ohne jeden Blockchain-Bezug.

## Ergänztes Beispiel (echter, getesteter Code)

Für einen reinen Dependency-Bump-PR besteht die eigentliche "Lösung" nicht aus neuem Code, sondern aus der Prüfung, dass es sich tatsächlich nur um einen sicheren Minor-/Patch-Bump handelt (kein versteckter Major-Versionswechsel mit Breaking Changes). Das lässt sich generisch, echt und korrekt prüfen:

```python
"""Reale, korrekte Beispielimplementierung: prueft, ob eine neue
Abhaengigkeits-Version (wie sie automatisierte Bots wie Renovate/Dependabot
vorschlagen) tatsaechlich neuer ist als die aktuell deklarierte Version -
das eigentliche, generalisierbare Muster hinter einem Dependency-Update-PR,
unabhaengig vom Paket-Oekosystem (npm, composer, cargo, ...)."""
from typing import Tuple


def parse_semver(version: str) -> Tuple[int, int, int]:
    """Zerlegt 'X.Y.Z' (optionale Pre-Release-/Build-Suffixe werden
    ignoriert) in ein vergleichbares Tupel."""
    core = version.split("-")[0].split("+")[0]
    parts = core.split(".")
    parts += ["0"] * (3 - len(parts))
    return tuple(int(p) for p in parts[:3])


def is_safe_minor_or_patch_bump(old_version: str, new_version: str) -> bool:
    """Prueft, ob `new_version` eine reine Minor- oder Patch-Anhebung
    gegenueber `old_version` ist (gleiche Major-Version, neue Version ist
    tatsaechlich groesser) - genau das, was ein "update all minor
    dependencies"-PR verspricht. Ein Major-Bump oder eine tatsaechlich
    KLEINERE Version wird korrekt abgelehnt."""
    old = parse_semver(old_version)
    new = parse_semver(new_version)
    if old[0] != new[0]:
        return False  # Major-Version-Wechsel ist KEIN "nur minor/patch"
    return new > old


if __name__ == "__main__":
    cases = [
        ("15.24.0", "15.26.0", True),   # echter Fall aus dem Original-PR
        ("3.356.21", "3.394.0", True),  # echter Fall aus dem Original-PR
        ("2.0.0", "3.0.0", False),      # Major-Bump - KEIN reiner minor/patch
        ("1.5.0", "1.4.9", False),      # tatsaechlich aeltere Version
    ]
    for old, new, expected in cases:
        result = is_safe_minor_or_patch_bump(old, new)
        status = "OK" if result == expected else "FEHLER"
        print(f"{old} -> {new}: sicherer minor/patch-Bump={result} (erwartet={expected}) [{status}]")
```



---

*Automatisiert erzeugt und archiviert; Code-Beispiel nachträglich manuell ergänzt.*
