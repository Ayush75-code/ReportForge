# Fix Scripts

One-time migration and fix scripts.

## Examples
- `migrate-data.ts` - Migrate data between schema versions
- `fix-null-values.ts` - Fix null values in existing records
- `backfill-scores.ts` - Backfill performance scores for old reports

## Usage
```bash
npx tsx scripts/fixes/your-script.ts
```

## Note
These scripts are typically run once and can be deleted after use.
