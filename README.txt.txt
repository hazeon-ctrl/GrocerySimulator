# Grocery Store Simulator - Developer Documentation

## Overview
A text-based grocery store management simulator built with vanilla HTML, CSS, and JavaScript. Players manage inventory, cashiers, pricing, and daily operations while maintaining store reputation and profitability.

## Core Game Architecture

### Game State Object
```javascript
gameState = {
    cash: 10000,           // Starting money
    day: 1,                // Current day counter
    reputation: 50,        // Store reputation (0-100)
    cashiers: 1,           // Number of hired cashiers
    departments: {         // Department data structure
        produce: {
            stock: 100,    // Current inventory units
            cost: 2,       // Base cost per unit
            margin: 1.5,   // Profit multiplier
            weight: 25     // Probability weight for customer demand
        }
        // ... more departments
    }
}
```

## Key Game Mechanics

### 1. Weighted Lottery System
The `weightedLottery()` function determines customer demand:
- Each department has a "weight" representing demand probability
- Higher weight = more likely to be selected by customers
- Uses cumulative probability distribution for realistic shopping patterns

```javascript
function weightedLottery(departments) {
    const totalWeight = sum of all department weights
    const random = Math.random() * totalWeight
    // Returns department name based on cumulative weight ranges
}
```

### 2. Customer Demand Calculation
Daily customer count formula:
```javascript
baseCustomers = 20 + (reputation/100) * 80  // 20-100 customers based on reputation
maxCapacity = cashiers * 50                 // Each cashier serves max 50 customers
actualCustomers = Math.min(baseCustomers + random(0-30), maxCapacity)
```

### 3. Reputation System
- **Gains**: +0.5 per successful sale
- **Losses**: -1.0 per stockout (customer can't find what they want)
- **Range**: 0-100 (affects customer count and game over conditions)

### 4. Pricing Model
Sale Price = Cost × Margin
- Cost: Base wholesale price per unit
- Margin: Multiplier (1.1x to 3.0x allowed)
- Higher margins = more profit but may reduce demand indirectly through reputation loss

## Core Functions

### Display Management
- `updateDisplay()`: Updates all UI elements with current game state
- `renderDepartments()`: Dynamically generates department management cards
- `logMessage()`: Adds timestamped messages to sales log with color coding

### Business Operations
- `buyInventory(deptName)`: Purchase stock for specific department
- `setMargin(deptName)`: Update profit margins
- `hireCashier()` / `fireCashier()`: Staff management

### Daily Simulation
The `endDay()` function is the core simulation engine:

1. **Expense Calculation**: Deduct daily wages
2. **Customer Generation**: Calculate total customers for the day
3. **Sales Simulation**: For each customer:
   - Use weighted lottery to determine wanted department
   - Generate random purchase quantity (1-5 units)
   - Check stock availability
   - Process sale or record stockout
4. **Financial Update**: Add revenue, update cash
5. **Reputation Update**: Apply changes based on success/failure ratio
6. **Logging**: Generate detailed daily report
7. **Game State Advance**: Increment day counter

### Profit Projection
Real-time net profit calculation considers:
- Daily wage expenses
- Projected customer count (reputation-based)
- Cashier capacity limits
- Department-specific success rates based on stock levels
- Weighted average of potential sales across all departments

## File Structure

### HTML Structure
- Statistics dashboard (cash, day, reputation)
- Cashier management panel
- Department management grid (dynamically generated)
- End day button with profit projection
- Sales log display area

### CSS Styling
- Retro terminal theme (green on black)
- Grid-based responsive layout
- Color-coded elements (success/warning/error states)
- Animated reputation bar

### JavaScript Organization
- Global game state object
- Initialization functions
- UI update functions
- Business logic functions
- Simulation engine (endDay)

## Game Balance

### Starting Conditions
- $10,000 starting cash
- 1 cashier (50 customer capacity)
- Moderate starting inventory across 6 departments
- 50/100 reputation (neutral)

### Economic Pressure
- Daily cashier wages create ongoing expenses
- Stockouts damage reputation and reduce future customers
- Over-hiring cashiers wastes money on unused capacity
- Under-stocking leads to lost sales

### Strategic Decisions
Players must balance:
- Inventory investment vs. cash reserves
- Staffing levels vs. daily expenses
- Profit margins vs. customer satisfaction
- Department focus vs. diversified approach

## Extension Points

### Adding New Features
- New departments: Add to gameState.departments with stock/cost/margin/weight
- Different cashier types: Modify hire/fire functions and capacity calculations
- Seasonal events: Modify weights or costs based on day counter
- Competitor actions: Add external factors affecting reputation/demand
- Marketing campaigns: Add reputation boost mechanics

### Code Modifications
- All department data is data-driven (easy to modify)
- Weighted lottery system can accommodate any number of departments
- Profit projection automatically adapts to game state changes
- Modular function structure allows easy feature additions

## Performance Considerations
- No external dependencies (runs entirely client-side)
- Minimal DOM manipulation (efficient updates)
- In-memory storage only (no localStorage used)
- Lightweight simulation (handles hundreds of customers efficiently)

## Browser Compatibility
- Uses ES6 features (const/let, arrow functions, template literals)
- CSS Grid and Flexbox for layout
- No external libraries required
- Works in all modern browsers