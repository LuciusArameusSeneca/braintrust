# Grant packet: ALLMIGHT microgrant — canonical identity + Activity Tape starter kit

**Datum:** 2026-09-13  
**Bewertung:** 85/100  
**Einordnung:** Zusaetzlich als moegliche Web3-Security-/Smart-Contract-Audit-Aufgabe erkannt (Chain: solana) - eine kuratierte, manuell gegengeprüfte Fassung findet sich ggf. im [security-portfolio](https://github.com/LuciusArameusSeneca/security-portfolio).

> ⚠️ **Unverifizierter, automatisiert erzeugter Eintrag.** Dieser Eintrag wurde OHNE manuelle Pruefung automatisch archiviert und kann Fehler oder erfundene Inhalte enthalten - insbesondere erfundenen Code, der auf nicht existierende Dateien/Funktionen verweist. Kein Ersatz fuer eine manuelle Verifikation.

---

## Kurzbeschreibung

## Funding Route

**ALLMIGHT Microgrants Program** on Superteam Earn

- United States only
- Up to 10,000 USDG
- About one week average response time
- 25% upfront, remaining funding after milestones
- Favors open-source Solana public goods, community infrastructure, creator work and practical commerce/onboarding experiments

Official application:

https://superteam.fun/earn/grants/allmight-microgrants

## Recommended Ask

**7,500 USDG over six weeks**

### Canonical Identity + Activity Tape Starter Kit
- **Solana Community Starter Kit: Canonical Identity + Activity Tape**
  - **$DASHA**: First reference deployment and one unrelated U.S. community as an external pilot.
  
---

This is a measured problem for `$DASHA`:
- The canonical mint has an active CoinGecko listing and correct GeckoTerminal takeover data, but remains unverified in major wallet surfaces; a separate same-name token appeared on August 30 using copied social context.

The full mint is the only stable identity.

## Ergebnis (unverifiziert)

**Lösungsansatz**

### Problemstellung und Lösungsstrategie

Der Solana Token `$DASHA` hat ein aktives CoinGecko-Verzeichnis mit korrekter GeckoTerminal-Takeover-Daten, aber der canonical mint (aktive Token) ist noch unverifiziert in den meisten Wallets und Screeners. Es existieren auch andere Tokens mit identischen Namen, Symbolen und sozialer Beschreibung.

### Schritt 1: Canonical Identity + Activity Tape Starter Kit

#### Solana Community Starter Kit
- **$DASHA**: Erste Referenzimplementierung und ein externer Pilot für eine unabhängige US-Kommunity.

### Schritt 2: Implementierung des Starter Kits

1. **Canonical Identity**:
   - Erstellen einer token-agnostischen Schema `solana.community-identity/v1` mit den folgenden Inhalten:
     - Chain und Netzwerk
     - Exakte Token-Mint-ID
     - Exacte canonical Pool oder Markt-Adressen, gekennzeichnet nach Veranstaltungsort
     - Projektname und Symbol als Anzeigefelder (nie primäre Identität)
     - Offizielle Website, soziale Medien, Repository- und Explorer-Routen
     - Bekannte CoinGecko/Jupiter/anderer Registrierungsidentifikatoren mit datierten Statusfeldern
     - Metadatenquelle und letzte Verifizierungstimestamp
   - Implementieren der JSON-Schema oder einer gleichwertigen ausführbaren Validierungs-API.
   - Erstellen von negativen Fixtures für Kollisionen mit gleichen Namen/Symbolen, substituierter Mint oder falscher Daten.

2. **Activity Tape**:
   - Erstellen einer menschenlesbaren Aktivitätsfeed mit `$DASHA` als Tokenname und Symbol.
   - Implementieren der API für das aktuelle canonical mint, um es in Screenern zu verifizieren.

3. **External Pilot**:
   - Verbinden des canonical mints mit einer externen US-Kommunity, um die Nutzung identischer Tokens zu verhindern.

### Schritt 3: Test und Validierung

1. **Wallet Integration**:
   - Prüfen der Wallet-Integration für die Verifizierung identischer Tokens.
   - Implementieren und testen einer API für das aktuelle canonical mint.

2. **Screener Integration**:
   - Prüfen der Screener-Integration für die Verifizierung identischer Tokens.
   - Implementieren und testen einer API für das aktuelle canonical mint.

3. **External Pilot Integration**:
   - Verbinden des canonical mints mit der externen US-Kommunity.
   - Implementieren und testen einer API für das aktuelle canonical mint.

### Schritt 4: Start des Starter Kits

1. **Canonical Identity**:
   - Erstellen des canonical mints mit `$DASHA` als Tokenname und Symbol.
   - Implementieren der API für das aktuelle canonical mint.

2. **Activity Tape**:
   - Erstellen des menschenlesbaren Aktivitätsfeeds.
   - Implementieren der API für das aktuelle canonical mint.

3. **External Pilot**:
   - Verbinden des canonical mints mit dem externen US-Pilotprojekt.
   - Implementieren und testen der API für das aktuelle canonical mint.

### Fazit

Das Starter Kit bietet eine robuste Lösung zur Verifizierung von Token-Identitäten und Aktivitätseinträgen auf Solana, wodurch Konsistenz in Wallets und Screenern gewährleistet wird.

---

*Automatisiert erzeugt und archiviert.*
