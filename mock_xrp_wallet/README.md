# Mock XRP Wallet (Android)

A local, offline wallet-tracking app for XRP — lets you store wallet records
and log simulated payments against them. It does not talk to the XRP Ledger
network; "mock" is accurate, not a disclaimer.

## What it does

- `MainActivity` lists saved wallets in a `RecyclerView`, backed by a live
  query (`LiveData<List<Wallet>>`) so the list updates automatically.
- Tap a wallet to open `WalletDetailActivity`: view/rename it, delete it
  (cascades its payments), or add a payment.
- `CreateAddWalletActivity` creates a wallet two ways: enter an existing
  key/seed manually, or generate a mock wallet with `UUID`-based fake
  key/seed values (no real XRPL keys are ever created). `TransactionActivity`
  logs a payment (amount, destination, memo, timestamped) against a wallet.
- Long-press a wallet on the main list to delete it and its payment history.

## Tech stack

Java, Android SDK, **Room** (`AppDatabase`, `WalletDao`, `PaymentDao`) for
local SQLite persistence, `RecyclerView`/`LiveData` for reactive lists.

## How it works

`Wallet` and `Payment` are Room `@Entity` classes — `Payment` has a
`@ForeignKey` on `Wallet.id` with `onDelete = CASCADE`, so deleting a wallet
automatically removes its payment history at the database level. All reads
go through DAO `LiveData` queries that the activities observe directly, and
writes run on `AppDatabase.databaseWriteExecutor` off the main thread. This
is a genuine local CRUD app, not hardcoded sample data.

## Build & run

Open the project root in Android Studio and run the `app` configuration on
an emulator or device (Gradle-managed, no extra setup required).
