# 🧾 Employee Payroll System (EPS)

A full-stack server-side web application to manage employee records and calculate monthly payroll — built with **Node.js**, **Express**, and **EJS**. Features a modern dark UI and one-click Excel export.

---

## 📸 UI Preview

| Page              | Description                                                  |
| ----------------- | ------------------------------------------------------------ |
| **Dashboard**     | Sidebar layout, 6 live stat cards, searchable employee table |
| **Add Employee**  | Clean form with icon-prefixed inputs and validation          |
| **Edit Employee** | Pre-filled update form                                       |
| **Excel Export**  | Styled `.xlsx` file with 2 sheets — records + summary        |

---

## 🗂️ Project Structure

```
payroll-app/
├── modules/
│   └── fileHandler.js      # Custom fs.promises read/write module
├── public/
│   └── style.css           # Dark fintech UI — Instrument Serif + Bricolage Grotesque
├── views/
│   ├── index.ejs           # Dashboard: sidebar, stat cards, live search, table
│   ├── add.ejs             # Add employee form
│   └── edit.ejs            # Edit employee form
├── employees.json          # JSON flat-file database
├── server.js               # Express app — all routes including /export
├── package.json
└── README.md
```

---

## ⚙️ Tech Stack

| Layer        | Technology                             |
| ------------ | -------------------------------------- |
| Runtime      | Node.js v18+                           |
| Framework    | Express.js v4                          |
| Templating   | EJS                                    |
| Excel Export | ExcelJS v4                             |
| Database     | `employees.json` (flat file)           |
| Styling      | Vanilla CSS — dark glassmorphism theme |

---

## 🚀 Getting Started

### 1. Extract and enter the folder

```bash
unzip payroll-app-final.zip
cd payroll-app
```

### 2. Install dependencies

```bash
npm install
```

### 3. Start the server

```bash
node server.js
```

### 4. Open in your browser

```
http://localhost:3000
```

---

## 🔌 Routes

| Method | Route         | Description                                   |
| ------ | ------------- | --------------------------------------------- |
| GET    | `/`           | Dashboard — all employees, stats, live search |
| GET    | `/add`        | Add Employee form                             |
| POST   | `/add`        | Submit new employee                           |
| GET    | `/edit/:id`   | Edit Employee form (pre-filled)               |
| POST   | `/edit/:id`   | Submit updated employee                       |
| GET    | `/delete/:id` | Delete employee by ID                         |
| GET    | `/export`     | Download payroll as `.xlsx` file              |

---

## 📊 Excel Export

Click **📊 Export Excel** in the dashboard topbar to download a fully formatted `.xlsx` file.

### Sheet 1 — Employee Payroll

- Indigo title banner with report name
- Auto-generated subtitle with date and employee count
- Styled column headers (white on dark indigo)
- Alternating row colors (white / light purple)
- Tax values in **red**, Net Salary in **bold green**
- Currency formatted as `₹#,##0.00`
- Totals row at the bottom with real calculated values

### Sheet 2 — Summary

| Row                  | Value |
| -------------------- | ----- |
| Total Employees      | Count |
| Active Departments   | Count |
| Total Basic Salary   | Sum   |
| Total Tax (12%)      | Sum   |
| Total Net Salary     | Sum   |
| Average Basic Salary | Mean  |

> **Note:** All values are pre-calculated by Node.js before writing to the file. ExcelJS does not evaluate formula strings at runtime, so real numbers are written directly — this ensures the file opens correctly in Excel, LibreOffice, and Google Sheets.

---

## 💰 Payroll Calculation Logic

All calculations are done dynamically — in EJS for the dashboard, and in Node.js for the export:

```
Tax        = Basic Salary × 0.12     (12%)
Net Salary = Basic Salary − Tax
```

Dashboard summary aggregates:

```
Total Basic  = Σ all salaries
Total Tax    = Total Basic × 0.12
Total Net    = Total Basic − Total Tax
Avg Salary   = Total Basic ÷ Total Employees
```

---

## 🗃️ Data Model

Each employee entry in `employees.json`:

```json
{
  "id": 1713830400000,
  "name": "Ravi Sharma",
  "gender": "Male",
  "department": "Engineering",
  "salary": 55000,
  "startDate": "2022-03-15"
}
```

> IDs are generated with `Date.now()` to guarantee uniqueness.

---

## ✅ Validation Rules

- Name cannot be empty or whitespace only
- Salary must be a non-negative number
- Department cannot be empty
- On failure, the form re-renders with an inline error message — no data is lost

---

## 🔍 Live Search

The search bar on the dashboard filters rows in real time without any page reload:

- Click the **🔍 icon** to expand the search input
- Type to filter by **name**, **department**, **gender**, or **start date**
- The **Total Employees** stat card and table count update live
- Press **Escape** to close and reset

---

## 🎨 UI Design

| Element    | Detail                                                                                    |
| ---------- | ----------------------------------------------------------------------------------------- |
| Theme      | Dark fintech / luxury dashboard                                                           |
| Fonts      | `Instrument Serif` (headings) · `Bricolage Grotesque` (body) · `JetBrains Mono` (numbers) |
| Layout     | Fixed sidebar + sticky topbar + scrollable content                                        |
| Stat Cards | 6 cards with color-coded left accent strips                                               |
| Table      | Alternating rows, monospace salary values, color-coded tax/net                            |
| Buttons    | Indigo gradient (Add) · Emerald outline (Export)                                          |
| Effects    | Noise texture overlay · Ambient radial blur blobs · `slideUp` entry animations            |

---

## 📦 Dependencies

| Package   | Version | Purpose                       |
| --------- | ------- | ----------------------------- |
| `express` | ^4.18.2 | Web server and routing        |
| `ejs`     | ^3.1.9  | Server-side HTML templating   |
| `exceljs` | ^4.4.0  | Excel `.xlsx` file generation |

```bash
npm install
```

> **Why not `xlsx` (SheetJS)?** SheetJS has unfixable prototype pollution vulnerabilities (`GHSA-4r6h-8v6p-xvw6`). ExcelJS is the safe alternative.

---

## 🧩 Custom Module — `fileHandler.js`

All file I/O is isolated in `modules/fileHandler.js` using `fs.promises`:

```js
const { read, write } = require("./modules/fileHandler");

const employees = await read(); // → array of employee objects
await write(employees); // → saves back to employees.json
```

## Both methods use `try/catch` to prevent server crashes on file errors.

---

## 👨‍💻 Author

Built as a project submission for **GLA University, Mathura**  
Subject: Server-Side Web Development — Node.js & Express

---

## 📄 License

For educational purposes only.
