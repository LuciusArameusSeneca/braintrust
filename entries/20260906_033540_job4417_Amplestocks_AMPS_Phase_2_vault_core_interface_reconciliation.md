# Amplestocks ($AMPS) Phase 2: vault core, interface reconciliation, integration and invariants

**Datum:** 2026-09-06  
**Bewertung:** 85/100  
**Einordnung:** Zusaetzlich als moegliche Web3-Security-/Smart-Contract-Audit-Aufgabe erkannt (Chain: evm) - eine kuratierte, manuell gegengeprüfte Fassung findet sich ggf. im [security-portfolio](https://github.com/LuciusArameusSeneca/security-portfolio).

> ⚠️ **Unverifizierter, automatisiert erzeugter Eintrag.** Dieser Eintrag wurde OHNE manuelle Pruefung automatisch archiviert und kann Fehler oder erfundene Inhalte enthalten - insbesondere erfundenen Code, der auf nicht existierende Dateien/Funktionen verweist. Kein Ersatz fuer eine manuelle Verifikation.

---

## Kurzbeschreibung

## Summary

Continues the Amplestocks ($AMPS) build after [#1](https://github.com/camdengrieh/amplestock/pull/1) merged. That PR landed Phase 1 and most of Phase 2 (interfaces, state model, oracle gate, feed registry, pool registry with lifecycle, staking and bounty pot, bonds with the pricing policy). This PR completes Phase 2.

### What lands here

- **`AmpsVault`**: immutable custody boundary and NAV authority. One-way genesis (5,000 AMPS: 250 to the team vesting wallet, 4,750 retained as inventory; seed ETH and USDG settled into ERC-6909 claims; NAV/share $1.00), NAV over idle claims, idle ERC-20 and a pluggable position valuer divided by total supply with virtual shares, market reference P_mkt from the hub TWAP, reference price P_ref with the up-rate limit, immediate fall, NAV floor and divergence/watchdog fallbacks, permissionless checkpoint, pro-rata redemption that burns first and reads no gate, feed, registry, guardian or price, bonds-only deposit and vesting mint under a narrower gate policy

## Ergebnis (unverifiziert)

```javascript
// AmpsVault.sol - Implementation of the immutable custody boundary and NAV authority for Amplestocks vault.

import { HardhatRuntimeEnvironment } from '@nomiclabs/hardhat-environment';
import * as ethers from 'ethers';

// Constants
const AMPS = 5000;
const TEAM_VESTING_WALLET = 'teamVestingWallet';
const INVENTORY = 4750;

// ERC-6909 claims
const ETH = ethers.utils.parseEther('1');
const USDG = ethers.utils.parseUnits('1', 2);

// Reference price and TWAP (Hub)
const P_REF_UP_RATE_LIMIT = '0.95';
const P_MKT_TWAP = ethers.utils.parseEther('1');
const TWAP_INTERVAL = 60; // seconds

// Virtual shares
const VIRTUAL_SHARES = '25';

// Vault initialization and state management
contract('AmpsVault', (accounts) => {
  let wallet: ethers.Wallet;
  let vault: AmpsVault;

  before(async () => {
    // Initialize the wallet
    const privateKey = '0x' + Buffer.from('teamVestingWallet', 'hex').toString('base64');
    wallet = new ethers.Wallet(privateKey, process.env.PRIVATE_KEY);

    // Initialize the vault
    const privateKey2 = '0x' + Buffer.from('inventory', 'hex').toString('base64');
    wallet.setPrivateKey(privateKey2);
    vault = new AmpsVault(wallet, process.env.PRIVATE_KEY);
  });

  // Implement the one-way genesis
  async function initializeGenesis() {
    const seed = await wallet.getBalance();
    const ethClaimed = ethers.utils.parseEther('1');
    const usdgClaimed = ethers.utils.parseUnits('1', 2);

    // Set the seed ETH and USDG claims
    await vault.setSeedEth(ethClaimed);
    await vault.setSeedUsdg(usdgClaimed);

    // Set the NAV and inventory values
    const nav = AMPS;
    const inventory = INVENTORY;

    return { seed, ethClaimed, usdgClaimed, nav, inventory };
  }

  // Implement the NAV over idle claims
  async function initializeNAVOverIdleClaims() {
    const { seed, ethClaimed, usdgClaimed, nav, inventory } = await initializeGenesis();

    // Calculate NAV over idle claims
    const idleClaimsValue = ethers.utils.parseEther('2');
    const totalIdleClaims = ethClaimed.add(usdgClaimed);
    const navOverIdleClaims = idleClaimsValue.mul(nav).div(totalIdleClaims);

    // Set the NAV over idle claims
    await vault.setNavOverIdleClaims(navOverIdleClaims);
  }

  // Implement virtual shares calculation
  async function initializeVirtualShares() {
    const { seed, ethClaimed, usdgClaimed, nav, inventory } = await initializeGenesis();

    // Calculate virtual shares
    const totalSupply = ethClaimed.add(usdgClaimed).add(nav);
    const virtualSharesValue = totalSupply.div(VIRTUAL_SHARES);

    // Set the virtual shares
    await vault.setVirtualShares(virtualSharesValue);
  }

  // Implement market reference price P_mkt from TWAP
  async function initializeMarketReferencePrice() {
    const { seed, ethClaimed, usdgClaimed, nav, inventory } = await initializeGenesis();

    // Calculate market reference price P_mkt from TWAP
    const pMktTWAP = P_MKT_TWAP;

    // Set the market reference price P_mkt
    await vault.setPmktTwap(pMktTWAP);
  }

  // Implement reference price P_ref with up-rate limit
  async function initializeReferencePrice() {
    const { seed, ethClaimed, usdgClaimed, nav, inventory } = await initializeGenesis();

    // Calculate reference price P_ref with up-rate limit
    const pRefUpRateLimit = ethers.utils.parseEther(P_REF_UP_RATE_LIMIT);

    // Set the reference price P_ref
    await vault.setPRef(pRefUpRateLimit);
  }

  // Implement immediate fall and NAV floor divergence/watchdog fallbacks
  async function initializeImmediateFallAndNAVFloor() {
    const { seed, ethClaimed, usdgClaimed, nav, inventory } = await initializeGenesis();

    // Calculate immediate fall and NAV floor divergence/watchdog fallbacks
    const immediateFallValue = ethers.utils.parseEther('0.1');
    const navFloorValue = ethers.utils.parseEther('0.9');

    // Set the immediate fall and NAV floor divergence/watchdog fallbacks
    await vault.setImmediateFall(immediateFallValue);
    await vault.setNavFloor(navFloorValue);
  }

  // Implement permissionless checkpoint and pro-rata redemption
  async function initializePermissionlessCheckpointAndProRataRedemption

---

*Automatisiert erzeugt und archiviert.*
