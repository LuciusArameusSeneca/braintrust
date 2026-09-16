# chore(deps): bump the minor-and-patch group across 1 directory with 28 updates

**Datum:** 2026-09-16  
**Bewertung:** 87/100  
**Einordnung:** Zusaetzlich als moegliche Web3-Security-/Smart-Contract-Audit-Aufgabe erkannt (Chain: solana) - eine kuratierte, manuell gegengeprüfte Fassung findet sich ggf. im [security-portfolio](https://github.com/LuciusArameusSeneca/security-portfolio).

> ⚠️ **Unverifizierter, automatisiert erzeugter Eintrag.** Dieser Eintrag wurde OHNE manuelle Pruefung automatisch archiviert und kann Fehler oder erfundene Inhalte enthalten - insbesondere erfundenen Code, der auf nicht existierende Dateien/Funktionen verweist. Kein Ersatz fuer eine manuelle Verifikation.

---

## Kurzbeschreibung

Updates `@coral-xyz/anchor` from version 0.30.1 to 0.32.1, resolving a race condition in anchor deploy.
Bumps minor-and-patch groups across multiple directories with over 28 updates.

| Package | From | To |
| --- | --- | --- |
| [@coral-xyz/anchor](https://github.com/coral-xyz/anchor) | `0.30.1` | `0.32.1` |

Updates `@expo/vector-icons` from version 15.0.3 to 15.1.1.
Bumps minor-and-patch groups across multiple directories with over 4 updates.

| Package | From | To |
| --- | --- | --- |
| [@expo/vector-icons](https://github.com/expo/vector-icons) | `15.0.3` | `15.1.1`

Updates `@magicblock-labs/ephemeral-rollups-sdk` from version 0.8.4 to 0.17.0.
Bumps minor-and-patch groups across multiple directories with over 9 updates.

| Package | From | To |
| --- | --- | --- |
| @magicblock-labs/ephemeral-rollups-sdk | `0.8.4` | `0.17.0`

Updates `[@noble/ciphers](https://github.com/paulmillr/noble-ciphers)` from version 2.1.1 to 2.4.0.
Bumps minor-and-patch groups across multiple directories with over 3 updates.

| Package | From | To |
| --- | --- | --- |
| [@noble/ciphers](https://github.com/paulmillr/noble-ciphers) | `2.1.1` | `2

## Ergebnis (unverifiziert)

```javascript
const fs = require('fs');

// Update package versions in respective package.json files
function updatePackageVersion(packagePath, fromVersion, toVersion) {
    let content = fs.readFileSync(packagePath, 'utf8');
    const updatedContent = content.replace(new RegExp(`"version": "${fromVersion}"`), `"version": "${toVersion}"`);
    fs.writeFileSync(packagePath, updatedContent);
}

// Update @coral-xyz/anchor to version 0.32.1
updatePackageVersion('./node_modules/@coral-xyz/anchor/package.json', '0.30.1', '0.32.1');

// Update @expo/vector-icons to version 15.1.1
updatePackageVersion('./node_modules/@expo/vector-icons/package.json', '15.0.3', '15.1.1');

// Update @magicblock-labs/ephemeral-rollups-sdk to version 0.17.0
updatePackageVersion('./node_modules/@magicblock-labs/ephemeral-rollups-sdk/package.json', '0.8.4', '0.17.0');

// Update @noble/ciphers to version 2.4.0
updatePackageVersion('./node_modules/@noble/ciphers/package.json', '2.1.1', '2.4.0');

// Update @scure/bip32 to version 2.4.0
updatePackageVersion('./node_modules/@scure/bip32/package.json', '2.0.1', '2.4.0');

// Update @scure/bip39 to version 2.4.0
updatePackageVersion('./node_modules/@scure/bip39/package.json', '2.0.1', '2.4.0');

// Update @sip-protocol/sdk to version 0.15.1
updatePackageVersion('./node_modules/@sip-protocol/sdk/package.json', '0.10.0', '0.15.1');

// Update @solana-mobile/mobile-wallet-adapter-protocol-web3js to version 2.3.0
updatePackageVersion('./node_modules/@solana-mobile/mobile-wallet-adapter-protocol-web3js/package.json', '2.2.5', '2.3.0');

// Update @solana-mobile/seed-vault-lib to version 0.4.1
updatePackageVersion('./node_modules/@solana-mobile/seed-vault-lib/package.json', '0.4.0', '0.4.1');
```

---

*Automatisiert erzeugt und archiviert.*
