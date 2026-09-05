# Add Oak Security to Filecoin

**Datum:** 2026-09-05  
**Bewertung:** 80/100  
**Einordnung:** Zusaetzlich als moegliche Web3-Security-/Smart-Contract-Audit-Aufgabe erkannt (Chain: evm) - eine kuratierte, manuell gegengeprüfte Fassung findet sich ggf. im [security-portfolio](https://github.com/LuciusArameusSeneca/security-portfolio).

> ⚠️ **Unverifizierter, automatisiert erzeugter Eintrag.** Dieser Eintrag wurde OHNE manuelle Pruefung automatisch archiviert und kann Fehler oder erfundene Inhalte enthalten - insbesondere erfundenen Code, der auf nicht existierende Dateien/Funktionen verweist. Kein Ersatz fuer eine manuelle Verifikation.

---

## Kurzbeschreibung

## What changed

- Added Oak Security as a provider with its official website, social, repository, and contact details.
- Added Oak Security's canonical Web3 security-audit offer.
- Added a Filecoin-specific listing linked to Filecoin's official FEVM audit record and Oak's published report.

## Why

Filecoin's official audit index records Oak Security's March 2023 audit of the Filecoin EVM implementation. The published scope covered `actors/evm`, `actors/eam`, `ref-fvm`, EVM opcodes, gas accounting, WASM integration, and related FEVM execution paths. Oak remains an active independent Web3 audit provider, so this fills a verified provider, offer, and Filecoin security-listing gap.

## Validation

- `python3 git-hooks/pre-commit.py` — all checks passed
- Parsed all three CSVs and verified the new rows match their 14/16/16-column schemas
- `git diff --check`
- Checked the current repository and open PR/issue queues for an existing Oak Security contribution

## Bounty note

Filecoin is currently eligible and security has a 1.5× weekly multiplier. The three rows contain 26 newly populated cells (up

## Ergebnis (unverifiziert)

```javascript
// JavaScript code for validating the new rows in CSV files

const fs = require('fs');
const csvParser = require('@grpc/proto-loader');

function validateCSVRows(filePath, expectedColumns) {
    const data = fs.readFileSync(filePath, 'utf8');
    
    // Parse the CSV file
    let parsedData;
    try {
        parsedData = csvParser.parseStringSync(data);
    } catch (error) {
        console.error(`Error parsing CSV file: ${error}`);
        return false;
    }
    
    // Check if the row count matches expected values
    const rowsCount = parsedData.length - 1; // Subtract header row
    
    // Check if each row has the expected number of columns
    for (let i = 0; i < rowsCount; i++) {
        if (parsedData[i].length !== expectedColumns) {
            console.error(`Row ${i + 1} has an incorrect number of columns`);
            return false;
        }
    }
    
    return rowsCount === expectedColumns;
}

// Example usage:
const filePaths = ['path/to/csv/file.csv', 'another/path/to/csv/file.csv'];
const expectedColumns = 16; // Adjust based on the actual number of columns
for (let i = 0; i < filePaths.length; i++) {
    const isValid = validateCSVRows(filePaths[i], expectedColumns);
    
    if (!isValid) {
        console.error(`Row validation failed for ${filePaths[i]}`);
    } else {
        console.log(`${filePaths[i]} is valid.`);
    }
}
```

### Erklärung:
- **JavaScript code**: Diese JavaScript-Funktion verwendet `fs` und `csvParser` für die CSV-Parse. Sie überprüft, ob das angegebene CSV-Dateiobjekt eine gültige Anzahl an Zeilen und Spalten enthält.
  
- **CSV-Parse**: Die Funktion `csvParser.parseStringSync` wird verwendet, um das CSV-Datenobjekt zu parsen.

- **Überprüfung**:
  - Die Funktion überprüft, ob das angegebene CSV-Datenobjekt eine gültige Anzahl an Zeilen enthält.
  - Sie überprüft auch, ob jede Zeile die erwartete Anzahl von Spalten hat. Wenn eine Zeile die falsche Anzahl von Spalten hat, wird ein Fehler ausgegeben.

- **Anwendung**: Diese Funktion kann verwendet werden, um sicherzustellen, dass die CSV-Datenobjekte in den angegebenen Dateien eine gültige Anzahl an Zeilen und Spalten enthalten.

---

*Automatisiert erzeugt und archiviert.*
