# Feat/auth phone otp verification rw ug

**Datum:** 2026-09-05  
**Bewertung:** 85/100  
**Einordnung:** Kein Security-/Audit-Bezug erkannt.

> ⚠️ **Automatisiert gefundener Auftrag, manuell überarbeitet.** Die Kurzbeschreibung stammt aus einer automatisierten, unverifizierten Erfassung und kann ungenau sein. Der ursprüngliche, automatisiert erzeugte "Lösungs"-Code war erfunden bzw. nicht lauffähig und wurde durch ein echtes, getestetes Beispiel ersetzt, das das zugrunde liegende technische Konzept korrekt demonstriert - nicht notwendigerweise eine exakte Lösung für den spezifischen Originalauftrag.

---

## Kurzbeschreibung

Pull-Request, der drei neue Telefon-OTP-Endpunkte hinzufügt (`POST /api/auth/otp/request`, `.../otp/verify`, `.../otp/resend`) in einem neuen `src/otp/`-Modul, isoliert von den bestehenden Auth-/E-Mail-Verifizierungs-Routen. Laut Beschreibung wurden 239 Backend-Routen auf mögliche Konflikte geprüft.

## Ergänztes Beispiel (echter, getesteter Code)

**Der reale, in der automatisierten Rohausgabe enthaltene Bug:** `verifyOtp()` erzeugte einen KOMPLETT NEUEN Zufalls-OTP und verglich diesen mit dem vom Nutzer eingegebenen Wert - zwei unabhängig gewürfelte Zufallswerte stimmen so gut wie nie überein, die Funktion hätte in der Praxis JEDE Verifizierung abgelehnt. Der korrekte Fix: den bei `requestOtp()` erzeugten Code tatsächlich speichern (mit Ablaufzeit) und bei `verifyOtp()` GENAU DIESEN gespeicherten Wert vergleichen. Lauffähig getestet mit Node.js:

```javascript
// otp_service.js - Reale, lauffaehige Korrektur des im Original-Auftrag
// gefundenen Bugs: verifyOtp() generierte einen NEUEN Zufalls-OTP und
// verglich ihn mit dem empfangenen Wert - das schlaegt praktisch IMMER
// fehl, weil zwei unabhaengig gewuerfelte Zufallswerte so gut wie nie
// uebereinstimmen. Der Fix: den bei requestOtp() erzeugten OTP tatsaechlich
// speichern (mit Ablaufzeit) und bei verifyOtp() GENAU DIESEN gespeicherten
// Wert vergleichen, statt einen neuen zu wuerfeln.

const crypto = require('crypto');

class OtpService {
  constructor(ttlMs = 5 * 60 * 1000) {
    this.ttlMs = ttlMs;
    this.store = new Map(); // phone -> { otp, expiresAt }
  }

  _generateOtp() {
    // 6-stelliger numerischer Code, kryptographisch zufaellig.
    return crypto.randomInt(0, 1000000).toString().padStart(6, '0');
  }

  requestOtp(phone) {
    const otp = this._generateOtp();
    this.store.set(phone, { otp, expiresAt: Date.now() + this.ttlMs });
    return otp; // in echt: per SMS versenden, nicht zurueckgeben
  }

  verifyOtp(phone, receivedOtp) {
    const entry = this.store.get(phone);
    if (!entry) return false;
    if (Date.now() > entry.expiresAt) {
      this.store.delete(phone);
      return false; // abgelaufen
    }
    const isValid = entry.otp === receivedOtp;
    if (isValid) this.store.delete(phone); // Einmalverwendung erzwingen
    return isValid;
  }

  resendOtp(phone) {
    return this.requestOtp(phone); // erzeugt bewusst einen NEUEN Code
  }
}

// --- Selbsttest: beweist, dass der Fix tatsaechlich funktioniert ---
function assert(condition, message) {
  if (!condition) throw new Error('FEHLGESCHLAGEN: ' + message);
  console.log('OK: ' + message);
}

const service = new OtpService();

const otp = service.requestOtp('+43111');
assert(service.verifyOtp('+43111', otp) === true, 'korrekter OTP wird akzeptiert');

const otp2 = service.requestOtp('+43222');
assert(service.verifyOtp('+43222', '000000') === false || otp2 === '000000',
  'falscher OTP wird abgelehnt (ausser Zufallstreffer 1:1.000.000)');

assert(service.verifyOtp('+43111', otp) === false,
  'derselbe OTP kann NICHT zweimal verwendet werden (Einmalverwendung)');

const otp3 = service.requestOtp('+43333');
service.store.get('+43333').expiresAt = Date.now() - 1; // simuliert Ablauf
assert(service.verifyOtp('+43333', otp3) === false, 'abgelaufener OTP wird abgelehnt');

console.log('\nAlle Pruefungen bestanden - im Gegensatz zur Original-Version, ' +
  'die JEDEN Verifizierungsversuch ablehnte (Selbstvergleich mit neu erzeugtem Wert).');
```



---

*Automatisiert erzeugt und archiviert; Code-Beispiel nachträglich manuell ergänzt.*
