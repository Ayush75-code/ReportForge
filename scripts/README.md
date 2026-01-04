# Scripts Directory

This folder contains utility scripts, database seeds, and one-time fix scripts.

**DO NOT put these scripts in the root directory or /src folder.**

## Folder Structure

```
/scripts
  /fixes      ← One-time migration/fix scripts
  /seeds      ← Database seed data
  /utils      ← Reusable utility scripts
  /tests      ← Manual test scripts
```

## Usage

Run scripts from the project root:

```bash
# Run a TypeScript script
npx ts-node scripts/fixes/migrate-data.ts

# Or with tsx (faster)
npx tsx scripts/seeds/seed-clients.ts
```

## Naming Convention

Use kebab-case for script files:
- `migrate-user-data.ts`
- `seed-test-clients.ts`
- `generate-types.ts`
