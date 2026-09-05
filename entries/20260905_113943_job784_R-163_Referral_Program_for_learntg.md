# R-#163: Referral Program for learn.tg

**Datum:** 2026-09-05  
**Bewertung:** 75/100  
**Einordnung:** Kein Security-/Audit-Bezug erkannt - allgemeiner GitHub-Auftrag.

> ⚠️ **Automatisierte Rohausgabe eines lokal/offline laufenden KI-Modells.** Dieser Eintrag wurde OHNE manuelle Pruefung automatisch archiviert und kann Fehler oder Halluzinationen enthalten - insbesondere erfundenen Code, der auf im Auftrag nicht existierende Dateien/Funktionen verweist. Kein Ersatz fuer eine manuelle Verifikation.

---

## Kurzerklaerung des Auftrags

Hier ist die Zusammenfassung der Krypto/Web3-Stellenausschreibung:

### Referral Program für Learn.tg

#### Beschreibung:
# R-#163: Referral Program for learn.tg

---

#### Implementierte Formen:
1. **Form 1 — Course Purchases:** 
   - The referrer earns 10% of the course price when a referred user buys any premium course (including Global Disciples).
2. **Form 2 — Missional Scholarships:**
   - The referrer earns 10% of the scholarship value when a referred user completes a missional course only "Una relación con Jesús".
3. **Form 3 — Pastor Bonus:** 
   - The referrer earns an additional $1 USDT when a referred pastor purchases the Global Disciples course.

---

#### Dedicated Referral Menu:
> **Separado (2026-08-29):** el **testimony system** se movió a **REQ/219**
> (privacidad/seudonimato, moderación, consentimiento) — no bloquea las recompensas de referidos. Referencia mutua: REQ/219 §8 fase

## Automatisierte KI-Loesung (unverifiziert)

**Loesungsentwurf für R-#163 Referral Program for learn.tg**

### Implementierte Formen:

#### 1. **Form 1 — Course Purchases:**
   ```javascript
   const coursePurchases = [
     { userReferrerId: 'user1', courseId: 'premium-course-1' },
     // weitere Referenten und Kurs-ID-Einträge
   ];
   
   const calculateReferralRewards = (coursePurchases) => {
     return coursePurchases.reduce((total, { userReferrerId }) => total + 0.1 * learn.tg.getCoursePrice(userReferrerId), 0);
   };
   
   const coursePurchasesRewards = calculateReferralRewards(coursePurchases);
   ```

#### 2. **Form 2 — Missional Scholarships:**
   ```javascript
   const missionalScholarshipRewards = [
     { userReferrerId: 'user1', scholarshipValue: 50 },
     // weitere Referenten und Schulforschung-Werte-Einträge
   ];
   
   const calculateReferralRewards = (missionalScholarshipRewards) => {
     return missionalScholarshipRewards.reduce((total, { scholarshipValue }) => total + 0.1 * scholarshipValue, 0);
   };
   
   const missionalScholarshipRewardsTotal = calculateReferralRewards(missionalScholarshipRewards);
   ```

#### 3. **Form 3 — Pastor Bonus:**
   ```javascript
   const pastorBonus = {
     userReferrerId: 'user1',
     coursePurchasedByPastor: true,
     bonusValue: 0.5
   };
   
   const calculateReferralRewards = (pastorBonus) => {
     return pastorBonus.bonusValue;
   };
   
   const totalPastorBonus = calculateReferralRewards(pastorBonus);
   ```

### Dedicated Referral Menu:
```javascript
const referralMenu = {
  code: 'user1',
  balance: totalReferralRewards,
  history: [],
  status: 'active'
};

// Tests für die Referral Menu-Implementierung
```

### Funding Rule:
```javascript
const fundingRule = {
  allRewards: totalReferralRewards,
  fromWallet: true,
  walletAddress: 'NEXT_PUBLIC_REFERRAL

---

*Automatisiert erzeugt und archiviert - dokumentiert einen real gefundenen GitHub-Auftrag.*
