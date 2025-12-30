# TaskGlitch - Bug Fixes Challenge

A Task Management Web App for sales teams to track, manage, and prioritize tasks based on ROI (Return on Investment).

## 🐛 Bugs Fixed

### BUG 1: Double Fetch Issue (API called twice on page load)
**Problem:** The app was loading task data twice on startup, causing duplicated entries.
**Root Cause:** Two `useEffect` hooks in `useTasks.ts` - one for initial load and another that appended more data after a timeout.
**Fix:** Removed the second `useEffect` to ensure data loads exactly once.

### BUG 2: Undo Snackbar Bug (Deleted task not cleared correctly)
**Problem:** When the undo snackbar closed without clicking undo, the `lastDeleted` state wasn't reset, causing future undo actions to restore old tasks.
**Root Cause:** Empty `handleCloseUndo` function in `App.tsx` that didn't reset state.
**Fix:** Added `resetLastDeleted` function to the tasks context and called it when the snackbar closes.

### BUG 3: Unstable Sorting (ROI ties cause flickering/reordering)
**Problem:** Tasks with the same ROI and priority would reorder randomly on each render, causing UI flickering.
**Root Cause:** `sortTasks` function used `Math.random()` as tie-breaker instead of stable sorting.
**Fix:** Replaced random tie-breaker with alphabetical sorting by task title.

### BUG 4: Double Dialog Opening (Edit/Delete triggers both View + Edit dialogs)
**Problem:** Clicking Edit or Delete buttons opened both the action dialog and the task details dialog.
**Root Cause:** Event bubbling - buttons were inside a clickable table row that also opened details.
**Fix:** Added `e.stopPropagation()` to button click handlers to prevent event bubbling.

### BUG 5: ROI Errors (Calculation & Validation Issues)
**Problem:** ROI showed "Infinity", "NaN", or blank values when time taken was 0 or inputs were invalid.
**Root Cause:** `computeROI` function didn't validate inputs for division by zero or non-finite numbers.
**Fix:** Added proper validation to return `null` for invalid cases (timeTaken ≤ 0 or non-finite numbers).

## 🚀 How to Run Locally

1. **Clone the repository:**
   ```bash
   git clone <repository-url>
   cd task-glitch
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Start the development server:**
   ```bash
   npm run dev
   ```

4. **Open your browser:**
   Navigate to `http://localhost:5173`

## 🌐 Live Deployment

The app is deployed and accessible at: [https://your-deployment-link.vercel.app](https://your-deployment-link.vercel.app)

## ✨ Features

- ✅ Add, edit, and delete tasks
- ✅ View task details and notes
- ✅ Search & filter by status and priority
- ✅ Calculate ROI = Revenue ÷ Time Taken
- ✅ Sort tasks by ROI and priority
- ✅ View summary insights (Total revenue, Efficiency, Average ROI, Performance grade)
- ✅ Import & export tasks via CSV
- ✅ Undo delete using a snackbar
- ✅ LocalStorage-based persistence

## 🛠️ Tech Stack

- **Frontend:** React 18, TypeScript
- **UI Library:** Material-UI (MUI)
- **Build Tool:** Vite
- **Charts:** MUI X Charts
- **State Management:** React Context + Hooks

## 📁 Project Structure

```
src/
├── components/          # Reusable UI components
├── context/            # React Context providers
├── hooks/              # Custom React hooks
├── types/              # TypeScript type definitions
└── utils/              # Utility functions and logic
```

## 🎯 Task Sorting Logic

Tasks are sorted with this priority:
1. **Primary:** ROI (descending)
2. **Secondary:** Priority (High > Medium > Low)
3. **Tie-breaker:** Alphabetical by title

## 📊 Metrics Calculated

- **Total Revenue:** Sum of all completed tasks' revenue
- **Time Efficiency:** Percentage of completed tasks
- **Revenue per Hour:** Total revenue ÷ total time taken
- **Average ROI:** Mean ROI across all tasks
- **Performance Grade:** Based on average ROI (Excellent/Good/Needs Improvement)

## 🔧 Extra Improvements Made

- Enhanced error handling in ROI calculations
- Improved sorting stability for better UX
- Fixed event handling to prevent unintended dialog openings
- Added proper state cleanup for undo functionality