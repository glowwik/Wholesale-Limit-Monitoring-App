# Wholesale Limit Monitoring App

**Volvo Financial Services** | Power Platform Solution

[![Power Apps](https://img.shields.io/badge/Power%20Apps-742774?style=flat&logo=powerapps&logoColor=white)](https://powerapps.microsoft.com/)
[![Power Automate](https://img.shields.io/badge/Power%20Automate-0066CC?style=flat&logo=powerautomate&logoColor=white)](https://flow.microsoft.com/)
[![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![SharePoint](https://img.shields.io/badge/SharePoint-0078D4?style=flat&logo=sharepoint&logoColor=white)](https://sharepoint.com/)

---

## 📋 Overview

The **Wholesale Limit Monitoring App** enables internal teams at Volvo Financial Services to:

- View dealer wholesale limits across subcategories (Stockplan New/Used, DDL, RV, etc.)
- Borrow limit between subcategories with a simple interface
- See live remaining balances via integrated Power BI data
- Maintain a complete audit trail of all limit adjustments

✅ **No more manual spreadsheets** | ✅ **Full compliance traceability** | ✅ **Real-time visibility**

---

## 🏗️ Architecture
Power BI (Credit Workspace)
↓
Power Automate Flows (scheduled/triggered)
↓
SharePoint Lists (Live Balance Storage)
↓
Power Apps (Canvas App)
↓
SharePoint Audit Log (every borrow action)


### Why this architecture?
Microsoft does not allow direct Power BI → Power Apps connections. The workaround uses Power Automate to extract Power BI data into SharePoint lists, which Power Apps can read.

---

## 📁 Components

| Component | Technology | Purpose |
|-----------|------------|---------|
| Front-end UI | Power Apps (Canvas) | Dealer selection, limit borrowing |
| Live balance data | Power BI report | Source of truth for exposure |
| Data bridge | Power Automate | Moves Power BI → SharePoint |
| Storage | SharePoint Lists | Stockplan Live Balance, DLL Live Balance |
| Audit trail | SharePoint List | `Audit - Entries Log - Limit Monitoring UKIE` |
| Solution container | Power Platform Solution | `Wholesale Limit Monitoring UKIE` |

---

## 🔧 SharePoint Lists Used

| List Name | Purpose |
|-----------|---------|
| `Stockplan Live Balance` | Cached live balance for Stockplan (New/Used) |
| `DLL Live Balance` | Cached live balance for DLL |
| `Audit - Entries Log - Limit Monitoring UKIE` | Every borrow action logged |
| `ORIGINAL Black folder - monitoring UKIE 2026` | Original records (reference) |
| `Black folder - monitoring UKIE 2026` | Consolidated current data |

---

## 🔄 Power Automate Flows

| Flow Name | Function |
|-----------|----------|
| `Get live balance for stockplan` | Pulls Stockplan data from Power BI → SharePoint |
| `Get live balance for DLL` | Pulls DLL data from Power BI → SharePoint |
| `Log Dealer Adjustment` | (Optional) Additional logging logic |

---

## 📱 App Walkthrough

### Step 1 – Front Page
Select a dealer from the dropdown.

### Step 2 – Borrow Interface
- **Left window:** Current limits per subcategory
- **Right window:** Borrow limit adjustment
- Enter amount to borrow from one category → add to another
- Click **Borrow** to execute

### Step 3 – Live Balance View
Scroll to see Power BI reports embedded showing:
- Stockplan (split into New vs Used)
- Remaining headroom

---

## 📊 Power BI Reports

| Report | Workspace | Filters |
|--------|-----------|---------|
| Live balance for app | Credit workspace | None (single table, created specifically for app) |
| CER | Credit workspace | Limit Monitoring App Tab, no filters |

---

## 🧪 Example User Flow

> User borrows **£100,000 from RV** and adds **£100,000 to DLL**  
> ✅ App updates current limits instantly  
> ✅ Audit record created in SharePoint with timestamp, dealer, amounts, user

---

## 🚀 How to Access

1. **Power Apps desktop version** (recommended)
2. **Front page of Credit SharePoint** – embedded link available

---

## 📸 Screenshots

| Screenshot | Description |
|------------|-------------|
| `screenshot-frontpage.png` | Dealer selection dropdown |
| `screenshot-borrow-interface.png` | Borrow window with left/right panes |
| `screenshot-pbi-reports.png` | Embedded Power BI reports showing live balance |
| `screenshot-sharepoint-audit.png` | Audit log entries in SharePoint |
| `screenshot-powerautomate-flows.png` | List of Power Automate flows |
| `screenshot-solution.png` | Solution view in Power Platform |

---

## ⚠️ Known Limitation

> Microsoft does not allow direct Power BI → Power Apps connectivity.  
> **Solution:** Power Automate flows run on schedule to sync Power BI data into SharePoint lists, which Power Apps reads.

---

## 👥 Stakeholders

| Role | Name |
|------|------|
| Responsible | Wiktoria Glowacka |
| Accountable | James Keeping |
| Consulted | Chris, Alison, Jayne |

---

## 📅 Version History

| Version | Date | Change | Modified by |
|---------|------|--------|-------------|
| 1 | 20/03/2026 | New | Wiktoria |

---

## 📄 License

Internal use – Volvo Financial Services

---

## 🙋‍♀️ Author

**Wiktoria Glowacka**  
Power Platform Developer

---

## 🔗 Related Links

- [Power Apps Documentation](https://learn.microsoft.com/en-us/power-apps/)
- [Power Automate Documentation](https://learn.microsoft.com/en-us/power-automate/)
- [Power BI Documentation](https://learn.microsoft.com/en-us/power-bi/)
