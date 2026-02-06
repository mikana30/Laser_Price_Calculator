# Laser Engraving Pricing Calculator

## Project Overview
Single-file browser-based pricing calculator for laser engraving businesses. Helps owners price products based on material costs, labor, overhead, and desired profit margins. Generates professional .docx client quotes.

## File Structure
- `laser-pricing-calculator.html` — The entire application (HTML + CSS + JS in one file, ~2,500 lines)
  - Lines 1-1131: CSS styles (dark theme, amber accents, responsive)
  - Lines 1133-1604: HTML markup (wizard, calculator, results, quote modal)
  - Lines 1606-2509: JavaScript (calculations, localStorage persistence, docx generation)

## Tech Stack
- Vanilla HTML/CSS/JS — no build tools or frameworks
- CDN dependencies: docx.js v7.8.2 (Word doc generation), FileSaver.js v2.0.5
- localStorage for preferences, history, and business info persistence
- No backend

## Key Concepts
- **Setup Wizard**: 3-step one-time config (hourly rate, overhead %, setup fee) saved to localStorage key `laserPricingPreferences`
- **Pricing Formula**: `costPerItem = material + (minutes/60 * hourlyRate) + (labor * overhead%) + (setupFee / qty)`
- **4 Markup Tiers**: 30% (entry), 50% (recommended), 75% (premium), 100% (luxury)
- **Bulk Discounts**: 4 configurable volume thresholds (5%/10%/15%/20% off)
- **Calculation History**: Up to 5 recent entries stored in localStorage key `laserPricingHistory`
- **Quote Generation**: Creates .docx with business info, line-item table, tax, and terms

## Conventions
- All monetary inputs use `$` prefix styling via `.input-with-prefix` / `.input-prefix`
- CSS uses custom properties defined in `:root` (--coal, --slate, --amber, etc.)
- UI state toggling uses `.hidden`, `.active`, `.open` CSS classes
- Global `currentCalculation` object stores last calc results for quote generation
- Global `window.currentChartData` stores data for the interactive profit chart

## Other Projects on D:\
- `FormFiller-master/` — Separate .NET 8 WPF app (police form auto-filler). Unrelated to this project.
