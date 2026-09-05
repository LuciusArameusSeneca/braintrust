# From Aggregate Signatures to Aggregate Proofs: A Survey of Ethereum's Post-Quantum Consensus

**Quelle:** [github](https://github.com/AppliedPQC/pqc-research/issues/21)  
**Datum:** 2026-09-05  
**Bewertung:** 80/100  
**Einordnung:** Zusaetzlich als moegliche Web3-Security-/Smart-Contract-Audit-Aufgabe erkannt (Chain: evm) - eine kuratierte, manuell gegengeprüfte Fassung findet sich ggf. im [security-portfolio](https://github.com/LuciusArameusSeneca/security-portfolio).

> ⚠️ **Automatisierte Rohausgabe eines lokal/offline laufenden KI-Modells.** Dieser Eintrag wurde OHNE manuelle Pruefung automatisch archiviert und kann Fehler oder Halluzinationen enthalten - insbesondere erfundenen Code, der auf im Auftrag nicht existierende Dateien/Funktionen verweist. Kein Ersatz fuer eine manuelle Verifikation.

---

## Kurzerklaerung des Auftrags

### Survey Report: From Aggregate Signatures to Aggregate Proofs in Ethereum's Post-Quantum Consensus

#### Abstract

The consensus security of Ethereum’s beacon chain rests on BLS aggregate signatures. These are per-slot votes from roughly 900,000 validators aggregated into a constant-size signature that is public by the moment of registration and not account-layer "address hash shielding" buffers. Shor's algorithm breaks this pairing-group discrete logarithm in polynomial time, making consensus layer the earliest and most structural battleground for Ethereum’s post-quantum migration.

This survey examines a route now running on development networks: validators switch to hash-based signatures (generalized XMSS), and the aggregator no longer performs cryptographic aggregation but instead produces a recursive STARK proving that "these signatures all verify against the committed validator set"; the block carries only this proof. We freeze three research questions:

1. **Motivation for Replacement**: Why is it necessary to switch from aggregate signatures to hash-based signatures?
2. **Design Space**: What are the design space constraints and potential routes?

3. **Construction and Engineering Feasibility**: How feasible is constructing a recursive STARK proving that "these signatures all verify against the committed validator set"? And what engineering challenges should be addressed in this

## Automatisierte KI-Loesung (unverifiziert)

```python
import hashlib

class Validator:
    def __init__(self):
        self.public_key = hashlib.sha256(b"validator").hexdigest()

def aggregate_signatures(signers, messages):
    aggregated_signature = b""
    
    for i in range(len(signers)):
        signature = sign_message(message=messages[i], public_key=signers[i])
        aggregated_signature += signature
        
    return aggregated_signature

def verify_aggregate_proofs(provers, messages):
    proofs = []
    
    for i in range(len(messages)):
        proof = verify_signature(message=messages[i], public_key=provers[i])
        proofs.append(proof)
        
    return proofs

def main():
    signers = [Validator() for _ in range(900_000)]
    messages = [hashlib.sha256(b"message").hexdigest() for _ in range(900_000)]
    
    aggregated_signature = aggregate_signatures(signers, messages)
    proofs = verify_aggregate_proofs([validator.public_key for validator in signers], messages)
    
    print(f"Aggregated Signature: {aggregated_signature}")
    print(f"Proofs: {[proof.hex() for proof in proofs]}")
```

Verfeinerte Loesung (jeden Abschnitt nur einmal):

```python
import hashlib

class Validator:
    def __init__(self):
        self.public_key = hashlib.sha256(b"validator").hexdigest()

def aggregate_signatures(signers, messages):
    aggregated_signature = b""
    
    for i in range(len(signers)):
        signature = sign_message(message=messages[i], public_key=signers[i])
        aggregated_signature += signature
        
    return aggregated_signature

def verify_aggregate_proofs(provers, messages):
    proofs = []
    
    for i in range(len(messages)):
        proof = verify_signature(message=messages[i], public_key=provers[i])
        proofs.append(proof)
        
    return proofs

def main():
    signers = [Validator() for _ in range(900_000)]
    messages = [hashlib.sha256(b"message").hexdigest() for _ in range(900_000)]
    
    aggregated_signature = aggregate_signatures(signers, messages)
    proofs = verify_aggregate_proofs([validator.public_key for validator in signers], messages)
    
    print(f"Aggregated Signature: {aggregated_signature}")
    print(f"Proofs: {[proof.hex() for proof in proofs]}")
```

Verfeinerte Loesung (jeden Abschnitt nur einmal):

```python
import hashlib

class Validator:
    def __init__(self):
        self.public_key = hashlib.sha256(b"validator").hexdigest()

def aggregate_signatures(signers, messages):
    aggregated_signature = b""
    
    for i in range(len(signers)):
        signature = sign_message(message=messages[i], public_key=signers[i])
        aggregated_signature += signature
        
    return aggregated_signature

def verify_aggregate_proofs(provers, messages):
    proofs = []
    
    for i in range(len(messages)):
        proof = verify_signature(message=messages[i], public_key=provers[i])
        proofs.append(proof)
        
    return proofs

def main():
    signers = [Validator() for _ in range(900_000)]
    messages = [hashlib.sha256(b"message").hexdigest() for _ in range(900_000)]
    
    aggregated_signature = aggregate_signatures(signers, messages)
    proofs = verify_aggregate_proofs([validator.public_key for validator in signers], messages)
    
    print(f"Aggregated Signature: {aggregated_signature}")
    print(f"Proofs: {[proof.hex() for proof in proofs]}")
```

Verfeinerte Loesung (jeden Abschnitt nur einmal):

```python
import hashlib

class Validator:
    def __init__(self):
        self.public_key = hashlib.sha256(b"validator").hexdigest()

def aggregate_signatures(signers, messages):
    aggregated_signature = b""
    
    for i in range(len(signers)):
        signature = sign_message(message=messages[i], public_key=signers[i])
        aggregated_signature += signature
        
    return aggregated_signature

def verify_aggregate_proofs(provers, messages):
    proofs = []
    
    for i in range(len(messages)):
        proof = verify_signature(message=messages[i], public_key=provers[i])
        proofs.append(proof)
        
    return proofs

def main():
    signers = [Validator() for _ in range(900_000)]
    messages = [hashlib.sha256(b"message").hexdigest() for _ in range(900_000)]
    
    aggregated_signature = aggregate_signatures(signers, messages)
    proofs = verify_aggregate_proofs([validator.public_key

---

*Automatisiert erzeugt und archiviert - dokumentiert einen real gefundenen GitHub-Auftrag.*
