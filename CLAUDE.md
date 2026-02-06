# Laser Engraving Pricing Calculator

## Project Overview
Single-file browser-based pricing calculator for laser engraving businesses. Helps owners price products based on material costs, labor, overhead, and desired profit margins. Generates professional .docx client quotes. Includes a customer database with linked quote tracking, autocomplete, and JSON import/export.

## File Structure
- `laser-pricing-calculator.html` — The entire application (HTML + CSS + JS in one file, ~3,600 lines)
  - Lines 1-1503: CSS styles (dark theme, amber accents, responsive, autocomplete, customer panel, data manager)
  - Lines 1505-2063: HTML markup (wizard, calculator, results, quote modal, customer panel modal, data manager modal)
  - Lines 2064-3589: JavaScript (calculations, localStorage persistence, docx generation, customer/quote CRUD, autocomplete, import/export)

## Tech Stack
- Vanilla HTML/CSS/JS — no build tools or frameworks
- Local vendor libs in `lib/`: docx.js v7.8.2 (Word doc generation), FileSaver.js v2.0.5
- localStorage for preferences, history, customers, quotes, and meta persistence
- No backend

## localStorage Keys
- `laserPricingPreferences` — Wizard settings (hourlyRate, overheadPercent, setupFee, includeSetupByDefault) + business info (businessName, businessEmail, businessPhone, businessAddress)
- `laserPricingHistory` — Array of up to 5 recent calculation entries
- `lpc_customers` — Array of customer objects `{ id, name, company, email, phone, address, notes, createdAt, updatedAt }`
- `lpc_quotes` — Array of quote objects `{ id, quoteNumber: "Q-00001", customerId, customerName, projectDescription, materialCost, minutes, setupFee, quantity, hourlyRate, overheadPercent, costPerItem, markupTier, pricePerItem, bulkDiscountPercent, subtotal, salesTaxRate, salesTax, totalPrice, status: "draft"|"sent"|"accepted"|"declined", createdAt, updatedAt }`
- `lpc_meta` — `{ nextCustomerId, nextQuoteId, dataVersion }` for auto-incrementing IDs

## Key Concepts
- **Setup Wizard**: 3-step one-time config (hourly rate, overhead %, setup fee) saved to localStorage key `laserPricingPreferences`
- **Pricing Formula**: `costPerItem = material + (minutes/60 * hourlyRate) + (labor * overhead%) + (setupFee / qty)`
- **4 Markup Tiers**: 30% (entry), 50% (recommended), 75% (premium), 100% (luxury)
- **Bulk Discounts**: 4 configurable volume thresholds (5%/10%/15%/20% off)
- **Calculation History**: Up to 5 recent entries stored in localStorage key `laserPricingHistory`
- **Quote Generation**: Creates .docx with business info, line-item table, tax, and terms. Saves quote snapshot to `lpc_quotes`.
- **Customer Database**: Customers auto-created from quote modal fields when generating a quote. Managed via Customer panel. Deleting a customer orphans their quotes (quotes preserved).
- **Autocomplete**: Client name field in quote modal has debounced autocomplete (150ms) with keyboard navigation (arrow keys, Enter, Escape). Searches by name+company, returns max 10 results.
- **Import/Export**: JSON export bundles all localStorage data into a versioned file. Import validates structure, confirms with user, replaces all data, reconciles ID counters.

## Function Groups
- **Persistence**: `loadPreferences/savePreferences`, `loadHistory/saveHistory`, `loadCustomers/saveCustomers`, `loadQuotes/saveQuotes`, `loadMeta/saveMeta`
- **Customer CRUD**: `addCustomer`, `updateCustomer`, `deleteCustomer`, `getCustomerById`, `searchCustomers`
- **Quote CRUD**: `addQuote`, `getQuotesForCustomer`, `updateQuoteStatus`
- **ID Management**: `getNextCustomerId`, `getNextQuoteId`, `formatQuoteNumber`
- **Utilities**: `escapeHtml`, `getStorageUsage`
- **Migration**: `migrateBusinessInfo` — moves old separate business info keys into preferences object
- **Autocomplete**: `initAutocomplete`, `renderAutocomplete`, `highlightMatch`, `closeAutocomplete`
- **Customer Panel**: `openCustomerPanel`, `closeCustomerPanel`, `showCustomerList`, `filterCustomerList`, `renderCustomerList`, `viewCustomer`, `editCustomerForm`, `saveCustomerEdit`, `deleteCurrentCustomer`
- **Data Manager**: `openDataManager`, `closeDataManager`, `updateStorageDisplay`, `exportAllData`, `triggerImport`, `importData`

## Conventions
- All monetary inputs use `$` prefix styling via `.input-with-prefix` / `.input-prefix`
- CSS uses custom properties defined in `:root` (--coal, --slate, --amber, etc.)
- UI state toggling uses `.hidden`, `.active`, `.open` CSS classes
- Global `currentCalculation` object stores last calc results for quote generation
- Global `window.currentChartData` stores data for the interactive profit chart
- Global `selectedCustomerId` tracks customer linked to current quote session
- Quote numbers are zero-padded sequential: `Q-00001`, `Q-00002`, etc.
- Quote status badges are color-coded: draft=silver, sent=blue, accepted=green, declined=red

## Other Projects on D:\
- `FormFiller-master/` — Separate .NET 8 WPF app (police form auto-filler). Unrelated to this project.
