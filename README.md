# Simple RevoGrid Calculator

A minimal SvelteKit application demonstrating RevoGrid with automatic math calculations.

## Features

- **Real RevoGrid Implementation** - Professional data grid component
- **Automatic Calculations** - Price × Quantity = Total (live updates)
- **Editable Cells** - Click price/quantity to edit values
- **Read-only Totals** - Calculated columns can't be manually edited
- **Mobile Responsive** - Works on desktop and mobile devices
- **Svelte 5 + Runes** - Latest Svelte with modern reactivity

## Tech Stack

- [SvelteKit](https://kit.svelte.dev/) - Full-stack Svelte framework
- [Svelte 5](https://svelte.dev/) - Latest version with runes
- [RevoGrid](https://rv-grid.com/) - High-performance data grid
- [TypeScript](https://www.typescriptlang.org/) - Type safety

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/ajonesb/simple-svelte-revogrid.git
   cd simple-svelte-revogrid
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Start the development server:
   ```bash
   npm run dev
   ```

4. Open your browser to `http://localhost:5173`

## Usage

- **Edit Values**: Click any Price or Quantity cell to modify
- **View Calculations**: Total column updates automatically
- **Summary**: See grand total and item count at the bottom
- **Mobile**: Fully responsive - test on your phone!

## Project Structure

```
src/
├── routes/
│   ├── +page.svelte      # Main RevoGrid component
│   ├── +page.ts          # Disables SSR for RevoGrid
│   └── +layout.svelte    # Layout with global styles
├── app.html              # HTML template
└── app.css               # Global styles and RevoGrid theme
```

## How Calculations Work

1. **User edits** a price or quantity cell
2. **RevoGrid fires** an `afteredit` event
3. **JavaScript calculates** `price × quantity = total`
4. **Svelte reactivity** updates the display instantly
5. **Summary totals** recalculate automatically

## Features Demonstrated

- Dynamic component loading (SSR-safe)
- Event handling with RevoGrid
- TypeScript interfaces for data
- Reactive calculations with Svelte 5 runes
- Custom column configuration
- Professional grid theming

## Mobile Support

The grid is fully responsive and touch-friendly. Test it on your mobile device for the complete experience.
