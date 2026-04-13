# Wholesale Limit Monitoring App

**Volvo Financial Services** | Power Platform Solution

[![Power Apps](https://img.shields.io/badge/Power%20Apps-742774?style=flat&logo=powerapps&logoColor=white)](https://powerapps.microsoft.com/)
[![Power Automate](https://img.shields.io/badge/Power%20Automate-0066CC?style=flat&logo=powerautomate&logoColor=white)](https://flow.microsoft.com/)
[![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![SharePoint](https://img.shields.io/badge/SharePoint-0078D4?style=flat&logo=sharepoint&logoColor=white)](https://sharepoint.com/)

---

## Overview

The **Wholesale Limit Monitoring App** enables internal teams at Volvo Financial Services to:

- View dealer wholesale limits across subcategories (Stockplan New/Used, DLL, RV, etc.)
- Borrow limit between subcategories with a simple interface
- See live remaining balances via integrated Power BI data
- Maintain a complete audit trail of all limit adjustments

**No more manual spreadsheets** | **Full compliance traceability** | **Real-time visibility**

<img width="1083" height="687" alt="Screenshot 2026-04-13 083942" src="https://github.com/user-attachments/assets/23adcb57-a37d-4012-adcb-7d3f53ada305" />

---

## Architecture
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

## Components

| Component | Technology | Purpose |
|-----------|------------|---------|
| Front-end UI | Power Apps (Canvas) | Dealer selection, limit borrowing |
| Live balance data | Power BI report | Source of truth for exposure |
| Data bridge | Power Automate | Moves Power BI → SharePoint |
| Storage | SharePoint Lists | `Stockplan Live Balance`, `DLL Live Balance` |
| Audit trail | SharePoint List | `Audit - Entries Log - Limit Monitoring UKIE` |
| Solution container | Power Platform Solution | `Wholesale Limit Monitoring UKIE` |

---

## SharePoint Lists Used

| List Name | Purpose |
|-----------|---------|
| `Stockplan Live Balance` | Cached live balance for Stockplan (New/Used) |
| `DLL Live Balance` | Cached live balance for DLL |
| `Audit - Entries Log - Limit Monitoring UKIE` | Every borrow action logged |
| `ORIGINAL Black folder - monitoring UKIE 2026` | Original records (reference) |
| `Black folder - monitoring UKIE 2026` | Consolidated current data |

---

## Power Automate Flows

| Flow Name | Function |
|-----------|----------|
| `Get live balance for stockplan` | Pulls Stockplan data from Power BI → SharePoint |
| `Get live balance for DLL` | Pulls DLL data from Power BI → SharePoint |
| `Log Dealer Adjustment` | (Optional) Additional logging logic |

---

## App Walkthrough

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

## Power BI Reports

| Report | Workspace | Filters |
|--------|-----------|---------|
| Live balance for app | Credit workspace | None (single table, created specifically for app) |
| CER | Credit workspace | Limit Monitoring App Tab, no filters |

---

##  Example User Flow

> User borrows **£100,000 from RV** and adds **£100,000 to DLL**  
>  App updates current limits instantly  
>  Audit record created in SharePoint with timestamp, dealer, amounts, user

---

## Screenshots

| Screenshot | Description |
|------------|-------------|
| <img width="1060" height="596" alt="image" src="https://github.com/user-attachments/assets/a37195bc-4d4e-4d03-940b-7fd1c05baff1" /> | Dealer selection dropdown |
| <img width="1055" height="594" alt="image" src="https://github.com/user-attachments/assets/9143ff13-e9c8-482b-84b0-9810507ef35a" /> | Borrow window with left/right panes |
| <img width="740" height="755" alt="image" src="https://github.com/user-attachments/assets/19138cb0-6cd0-40aa-84d3-c1c856a59ae4" /> <img width="882" height="469" alt="image" src="https://github.com/user-attachments/assets/5437cd4b-0893-4f89-bd7b-91c3faac546a" /> | Embedded Power BI reports showing live balance |
| <img width="1494" height="245" alt="image" src="https://github.com/user-attachments/assets/b49e111b-fbd1-49ce-bb30-7fcbf8fea81e" /> | Audit log entries in SharePoint |
| <img width="828" height="179" alt="image" src="https://github.com/user-attachments/assets/1ec3b81e-ff79-42c0-8f59-9868ab59def1" /> | List of Power Automate flows |
| <img width="873" height="445" alt="image" src="https://github.com/user-attachments/assets/030304b8-fd8f-4a90-a28d-a3d3d728f03c" /> | Solution view in Power Platform |

---

## Known Limitation

> Microsoft does not allow direct Power BI → Power Apps connectivity.  
> **Solution:** Power Automate flows run on schedule to sync Power BI data into SharePoint lists, which Power Apps reads.


## Terminology & Abbreviations

| Term | Description |
|------|-------------|
| **Stockplan New** | Wholesale financing for **new** dealer inventory. Dealers borrow against new vehicles on their lot. |
| **Stockplan Used** | Wholesale financing for **used** dealer inventory. Separate limit from new vehicles. |
| **DLL** | Dealer Lease Line – financing line for dealer's **leasing operations** (not vehicle inventory). |
| **RV** | Residual Value – financing related to lease/contract end values. Borrowing from RV means shifting limit from residual value pool to another category. |
| **Borrow** | Transfer limit from one subcategory to another. Example: Borrow £100k from RV → add to DLL. Total limit stays the same; allocation changes. |
| **Headroom** | Remaining available limit = (Current Limit) - (Current Exposure). Green = good. Red = over limit. |
| **CER** | Credit Exposure Report – Power BI dashboard showing real-time exposure across all dealers. |
| **Live Balance** | Real-time exposure data from Power BI. App shows cached version (max 15 min delay due to Microsoft limitation). |

## Author

**Wiktoria Glowacka**  
Power Platform Developer


## 🔗 Related Links

- [Power Apps Documentation](https://learn.microsoft.com/en-us/power-apps/)
- [Power Automate Documentation](https://learn.microsoft.com/en-us/power-automate/)
- [Power BI Documentation](https://learn.microsoft.com/en-us/power-bi/)
