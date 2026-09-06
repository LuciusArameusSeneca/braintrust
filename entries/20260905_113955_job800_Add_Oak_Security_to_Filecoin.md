# Add Oak Security to Filecoin

**Datum:** 2026-09-05  
**Bewertung:** 80/100  
**Einordnung:** Zusaetzlich als moegliche Web3-Security-/Smart-Contract-Audit-Aufgabe erkannt (Chain: evm) - eine kuratierte, manuell gegengeprüfte Fassung findet sich ggf. im [security-portfolio](https://github.com/LuciusArameusSeneca/security-portfolio).

> ⚠️ **Automatisiert gefundener Auftrag, manuell überarbeitet.** Die Kurzbeschreibung stammt aus einer automatisierten, unverifizierten Erfassung und kann ungenau sein. Der ursprüngliche, automatisiert erzeugte "Lösungs"-Code war erfunden bzw. nicht lauffähig und wurde durch ein echtes, getestetes Beispiel ersetzt, das das zugrunde liegende technische Konzept korrekt demonstriert - nicht notwendigerweise eine exakte Lösung für den spezifischen Originalauftrag.

---

## Kurzbeschreibung

Pull-Request, der den unabhängigen Web3-Audit-Anbieter "Oak Security" als neuen Eintrag in ein Verzeichnis von Security-Anbietern aufnimmt, inklusive eines Filecoin-spezifischen Listing-Eintrags (bezogen auf Oaks März-2023-Audit der Filecoin-EVM-Implementierung: `actors/evm`, `actors/eam`, `ref-fvm`). Erfordert laut Beschreibung u. a. das Einhalten eines festen CSV-Spaltenschemas (14/16/16 Spalten) für die drei betroffenen Tabellen.

## Ergänztes Beispiel (echter, getesteter Code)

Die automatisierte Rohausgabe importierte eine nicht existierende Bibliothek (`@grpc/proto-loader`) als angeblichen CSV-Parser - diese Bibliothek dient tatsächlich der gRPC-Protokolldefinition und hat mit CSV nichts zu tun. Eine echte, korrekte Spaltenanzahl-Prüfung mit der eingebauten `csv`-Bibliothek sieht so aus:

```python
"""Reale, korrekte CSV-Schema-Pruefung mit Pythons eingebautem csv-Modul -
ersetzt die im Original-Auftrag erfundene Bibliothek '@grpc/proto-loader'
(die kein CSV-Parser ist und nie existiert hat)."""
import csv
import io
from typing import List


def validate_csv_rows(csv_text: str, expected_columns: int) -> List[str]:
    """Prueft jede Datenzeile (ohne Kopfzeile) auf die erwartete
    Spaltenanzahl. Gibt eine Liste menschenlesbarer Fehlermeldungen zurueck
    (leer = alles gueltig)."""
    reader = csv.reader(io.StringIO(csv_text))
    rows = list(reader)
    if not rows:
        return ["CSV ist leer - keine Kopfzeile gefunden."]

    errors = []
    for i, row in enumerate(rows[1:], start=1):
        if len(row) != expected_columns:
            errors.append(
                f"Zeile {i}: {len(row)} Spalten gefunden, {expected_columns} erwartet."
            )
    return errors


if __name__ == "__main__":
    valid_csv = (
        "name,website,type\n"
        "Oak Security,https://oaksecurity.io,audit-provider\n"
        "Example Corp,https://example.com,audit-provider\n"
    )
    invalid_csv = (
        "name,website,type\n"
        "Oak Security,https://oaksecurity.io\n"  # fehlende Spalte
    )

    print("Gueltige CSV:", validate_csv_rows(valid_csv, expected_columns=3))
    print("Ungueltige CSV:", validate_csv_rows(invalid_csv, expected_columns=3))

    assert validate_csv_rows(valid_csv, 3) == []
    assert len(validate_csv_rows(invalid_csv, 3)) == 1
    print("Alle Pruefungen bestanden.")
```



---

*Automatisiert erzeugt und archiviert; Code-Beispiel nachträglich manuell ergänzt.*
