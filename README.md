# Monsoon Contracts

Smart contracts for **Monsoon** — compliant trade receivables financing for Asia's SME exporters, verified end-to-end with [Cleanverse](https://cleanverse.com)'s CVI (Verified Identity) and CVA (Verified Assets) infrastructure.

Companion app repository: `monsoon-app`.

Built for the **Cleanverse Build: Trusted Assets Hackathon** (RWA Track).

## Contracts

| Contract | File | Responsibility |
|---|---|---|
| **ComplianceRegistry** | `contracts/ComplianceRegistry.sol` | CVI gate. Tracks which wallets are CVI-verified, until when, and in what role (exporter / buyer / bank / admin). Every other contract checks this before minting, transferring, or financing anything. |
| **InvoiceAsset** | `contracts/InvoiceAsset.sol` | Tokenizes a buyer-confirmed receivable as an ERC-721. One invoice = one NFT, carrying amount, due date, both parties, and financing status. Transfer is blocked unless the recipient is CVI-verified — the rule lives inside the token logic itself. |
| **FinancingPool** | `contracts/FinancingPool.sol` | Financing business logic and the pool itself: takes the NFT as collateral, disburses the advance, collects repayment, releases the NFT back to the exporter. |
| **MockUSDC** (`contracts/mocks/`) | Test-mode settlement currency. Production settlement uses Cleanverse's real A-Token (CVA) instead — see `docs/CONTRACTS.md` §6. |

See [`docs/CONTRACTS.md`](docs/CONTRACTS.md) for the full architecture write-up (in Chinese) — call flow, dependency diagram, and how a receivable moves through all three contracts from mint to settlement.

## Deployed — Monad Testnet (chainId 10143)

| Contract | Address |
|---|---|
| ComplianceRegistry | [`0xec97185d251cd17e58a4d5b6bc0ac1df8d9056e7`](https://testnet.monadexplorer.com/address/0xec97185d251cd17e58a4d5b6bc0ac1df8d9056e7) |
| InvoiceAsset | [`0x756fc8f842ae808ad5af5a773e52580b2969e343`](https://testnet.monadexplorer.com/address/0x756fc8f842ae808ad5af5a773e52580b2969e343) |
| FinancingPool | [`0x6d210698924c56cc006c1f3d039bffdf607bfa4c`](https://testnet.monadexplorer.com/address/0x6d210698924c56cc006c1f3d039bffdf607bfa4c) |
| Settlement stablecoin (Cleanverse A-Token) | [`0x8602d979Accfa6c089fEc00519891B475c4A4412`](https://testnet.monadexplorer.com/address/0x8602d979Accfa6c089fEc00519891B475c4A4412) |

Deployment logs: [`output.md`](output.md), [`output/`](output/).

## Getting started

```bash
npm install
cp .env.example .env   # fill in RPC URL / deployer key for the network you're targeting
npx hardhat test
npx hardhat compile
```

## Live app

- App repo: `monsoon-app`
- Live demo: https://monsoon-app-alpha.vercel.app/
