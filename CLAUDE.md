# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

La Calculatrice is a minimalist financial calculator web app with two calculator types:
- **Loan Calculator**: Monthly payment calculations based on loan amount, interest rate, and term
- **RRSP/Investment Calculator**: Retirement savings projections with compound growth

The app allows users to add multiple calculators of either type to compare scenarios side-by-side.

## Commands

```bash
# Development
npm run dev          # Start dev server at http://localhost:3000

# Production
npm run build        # Build for production
npm run start        # Start production server

# Code Quality
npm run lint         # Run ESLint
```

## Development Guidelines

### Server Actions

**Always use Next.js Server Actions for any server-side operations.** When adding features that require:
- Data mutations
- Database operations
- API calls to external services
- Server-side data processing

Create server actions in separate files with the `'use server'` directive and import them into client components. Do not use API routes (`app/api/*`) unless absolutely necessary for webhook endpoints or third-party integrations.

## Architecture

### Application Structure

This is a Next.js 16 App Router application with a simple, flat structure:

- `app/page.tsx` - Single-page application containing all calculator logic
- `app/layout.tsx` - Root layout with IBM Plex Mono font configuration
- `components/ui/` - Radix UI components (unused in main app, part of shadcn/ui setup)
- `lib/utils.ts` - Utility functions (cn for class merging, formatDate, absoluteUrl)

### State Management

All state is managed locally in `app/page.tsx` using React useState:
- `calculators` array holds all calculator instances (type: `Calculator[]`)
- Each calculator has a unique `id` (timestamp) and `type` ('loan' | 'investment')
- Calculator updates trigger immediate recalculation via `calculateLoan()` or `calculateInvestment()`

### Calculator Types

**LoanCalculator Interface:**
- Inputs: `loanAmount`, `interestRate`, `loanTerm` (years)
- Outputs: `monthlyPayment`, `totalPayment`
- Formula: Standard amortization formula with monthly compounding

**InvestmentCalculator Interface:**
- Inputs: `currentAge`, `retirementAge`, `currentBalance`, `monthlyContribution`, `annualReturn`
- Outputs: `totalContributions`, `totalValue`, `growth`
- Formula: Future value of current balance + future value of annuity

### Styling

- Tailwind CSS with brutalist design aesthetic (black borders, uppercase text, monospace font)
- Uses IBM Plex Mono font loaded via next/font
- Responsive grid layout (1 column mobile, 2 columns tablet, 3 columns desktop)

### Key Behaviors

- Calculators can be duplicated (loan calculators preserve settings, RRSP uses defaults)
- Loan amount and investment fields support keyboard shortcuts (Arrow Up/Down)
- Input formatting: Currency fields display with thousands separators but store as numbers
- All calculations update immediately on input change (no submit button)

## Tech Stack

- Next.js 16 (App Router)
- React 19
- TypeScript 5
- Tailwind CSS 3
- Radix UI (via shadcn/ui components setup, though not actively used in main calculator)
