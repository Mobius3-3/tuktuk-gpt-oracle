# Tuktuk GPT Oracle

> Integration tests and helpers for the Tuktuk GPT Oracle example (Solana / Anchor).

This project contains tests that exercise an on-chain integration between a Solana program and the Tuktuk task queue plus an external LLM oracle. The tests live in `tests/tuktuk-gpt-oracle.ts` and use a devnet oracle/task-queue by default.

## Prerequisites

- Node.js (16+)
- npm or yarn
- Rust and Cargo
- Anchor CLI (anchor)
- Solana CLI configured for devnet

## Setup

1. Install JS deps:

```bash
npm install
# or
yarn install
```

2. Build the Anchor program(s) if you changed any on-chain code:

```bash
anchor build
```

3. Ensure your Solana CLI is pointed at the cluster you intend to use (devnet is used in the tests):

```bash
solana config set --url https://api.devnet.solana.com
```

## Tests

Run the integration tests with Anchor:

```bash
anchor test
```

The tests file is: [tests/tuktuk-gpt-oracle.ts](week2/tuktuk-gpt-oracle/tests/tuktuk-gpt-oracle.ts)

Important constants used by the tests (edit in the file if you want different deployments):

- `ORACLE_PROGRAM_ID` — the on-chain oracle program public key used for PDAs and context.
- `TUKTUK_PROGRAM_ID` — the TukTuk program id used to derive task PDAs.
- `TASK_QUEUE` — the task queue public key used to schedule tasks.

The tests include a helpful comment showing how to create a task queue with the `tuktuk` CLI, for example:

```text
tuktuk -u https://api.devnet.solana.com -w ~/.config/solana/id.json \
  task-queue create --name "tuktuk-gpt-oracle" --capacity 100 \
  --min-crank-reward 1000000 --funding-amount 500000000 --stale-task-age 60400
```

## Notes

- The tests assume an existing oracle program and task queue on the cluster; if you want a fully local flow you'll need to deploy those programs and update the constants in the tests file.
- The tests use `SYSTEM_PROGRAM_ID` and Anchor provider wallet as the payer — ensure your wallet is funded on the target cluster.

## Useful commands

- Run tests: `anchor test`
- Build program: `anchor build`
- Run a single test file (via mocha): `npx mocha -r ts-node/register tests/tuktuk-gpt-oracle.ts`
