# moment-1.7.2.min.js: 4 vulnerabilities (highest severity is: 8.7)

**Datum:** 2026-09-05  
**Bewertung:** 85/100  
**Einordnung:** Zusaetzlich als moegliche Web3-Security-/Smart-Contract-Audit-Aufgabe erkannt (Chain: evm) - eine kuratierte, manuell gegengeprüfte Fassung findet sich ggf. im [security-portfolio](https://github.com/LuciusArameusSeneca/security-portfolio).

> ⚠️ **Automatisiert gefundener Auftrag, manuell überarbeitet.** Die Kurzbeschreibung stammt aus einer automatisierten, unverifizierten Erfassung und kann ungenau sein. Der ursprüngliche, automatisiert erzeugte "Lösungs"-Code war erfunden bzw. nicht lauffähig und wurde durch ein echtes, getestetes Beispiel ersetzt, das das zugrunde liegende technische Konzept korrekt demonstriert - nicht notwendigerweise eine exakte Lösung für den spezifischen Originalauftrag.

---

## Kurzbeschreibung

Automatisierter Sicherheits-Scan meldet 4 Schwachstellen in einer veralteten moment.js-Version (u. a. CVE-2022-31129, ReDoS, Schweregrad 8.7) in `/app/assets/javascripts/moment.js`, installierte Version 2.29.4 wird als betroffen gemeldet, gepatchte Version ist Commit `b5cfc4221340ef65eb5bc7ae2c60bc1ae31a20ff`.

## Ergänztes Beispiel (echter, getesteter Code)

**Der eigentliche, richtige Fix für eine bekannte CVE in einer Abhängigkeit ist fast immer nur ein Versions-Update** (`npm install moment@latest` bzw. Pin auf die gepatchte Version) - NICHT das Schreiben eines eigenen "Vulnerability-Scanners" mit erfundenen APIs, wie es die automatisierte Rohausgabe tat. Ein echtes, minimales Skript, das genau das automatisiert prüft:

```javascript
// check_moment_version.js - Realer, korrekter Fix fuer den Original-
// Auftrag (moment.js CVE-2022-31129, ReDoS-Schwachstelle): der einzig
// richtige Fix ist ein simples Versions-Update der Abhaengigkeit, NICHT
// (wie im hallucinierten Original-Code) ein selbstgebauter "Vulnerability
// Scanner" mit erfundenen APIs. Dieses Skript prueft nur, ob die
// installierte Version bereits den gepatchten Mindeststand erreicht.

const MIN_SAFE_VERSION = [2, 29, 4]; // moment@2.29.4 behebt CVE-2022-31129

function parseVersion(version) {
  return version.split('.').map(Number);
}

function isVersionAtLeast(version, minVersion) {
  const v = parseVersion(version);
  for (let i = 0; i < minVersion.length; i++) {
    if ((v[i] || 0) > minVersion[i]) return true;
    if ((v[i] || 0) < minVersion[i]) return false;
  }
  return true;
}

function checkMomentVersion(packageJson) {
  const declared = packageJson.dependencies?.moment || packageJson.devDependencies?.moment;
  if (!declared) return { ok: true, reason: 'moment ist keine Abhaengigkeit' };

  const version = declared.replace(/^[\^~]/, '');
  const safe = isVersionAtLeast(version, MIN_SAFE_VERSION);
  return {
    ok: safe,
    version,
    reason: safe
      ? `moment@${version} ist bereits >= 2.29.4 (CVE-2022-31129 behoben)`
      : `moment@${version} ist VERWUNDBAR (CVE-2022-31129) - update auf mindestens 2.29.4 (z.B. "npm install moment@latest")`,
  };
}

// --- Selbsttest ---
const vulnerable = checkMomentVersion({ dependencies: { moment: '^2.24.0' } });
console.log('Verwundbare Version:', vulnerable);
if (vulnerable.ok) throw new Error('haette als verwundbar erkannt werden muessen');

const patched = checkMomentVersion({ dependencies: { moment: '^2.29.4' } });
console.log('Gepatchte Version:', patched);
if (!patched.ok) throw new Error('haette als sicher erkannt werden muessen');

console.log('\nAlle Pruefungen bestanden.');
```



---

*Automatisiert erzeugt und archiviert; Code-Beispiel nachträglich manuell ergänzt.*
