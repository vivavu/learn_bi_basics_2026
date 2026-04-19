# Prerequisites & Setup

Before starting the course, ensure your environment is properly configured. This guide covers software, licensing, and data access requirements.

---

## 🖥️ Software Requirements

### Power BI Desktop (Required)
- **Download:** [Power BI Desktop (Microsoft Store or web)](https://www.microsoft.com/en-us/download/details.aspx?id=58494)
- **Minimum Version:** Power BI Desktop (latest version recommended)
- **Size:** ~250 MB
- **OS:** Windows 7 or later (or Mac with M1/M2 via Parallels Desktop)
- **Verification:** Open Power BI Desktop → File → Help → About (check version)

**ℹ️ Note:** Power BI Desktop is FREE. You only need a license to *publish* dashboards to the cloud (Power BI Service).

### Microsoft Excel (Optional but Recommended)
- For preliminary data exploration and transformation
- If unavailable, Power Query (built into Power BI) handles all transformations

### SQL Client (Optional)
- If connecting to SQL Server data sources
- Examples: SQL Server Management Studio (SSMS), Azure Data Studio
- Not required for this course (we use CSV/Excel data)

---

## 📋 Licensing & Access

### Power BI Desktop (Local Work)
✅ **FREE** - Build dashboards locally without any subscription

### Power BI Service (Publishing & Sharing)
⚠️ **Requires License** - To publish dashboards to the cloud
- **Pro License:** $10/month per user (team sharing)
- **Premium License:** Enterprise-grade
- **Desktop-Only Alternative:** Save .pbix file locally and share via email/OneDrive (recommended for learning)

**Recommendation for this course:** Use **Power BI Desktop only** (free). Publish to cloud later once you're familiar with the tool.

---

## 📊 Data Access

### Sample Data (Included)
This course uses the **Czech Financial Dataset (AI4FCF)**:
- **Source:** https://sites.google.com/view/ai4fcf/open-datasets
- **Period:** 1993-1998
- **Records:** ~1M transactions, 4,500 accounts, 5,369 clients
- **Format:** CSV files
- **Access:** Public/open data (no authentication required)

### Download Data
1. Visit [AI4FCF Open Datasets](https://sites.google.com/view/ai4fcf/open-datasets)
2. Download Czech bank dataset (follow repo instructions for which files to use)
3. Store in `data/` folder (create if needed)
4. Verify files are readable in Excel before proceeding

---

## ✔️ Pre-Course Checklist

Before starting, verify the following:

- [ ] Power BI Desktop installed and latest version
- [ ] Can open Power BI Desktop without errors
- [ ] Czech financial dataset downloaded (if using provided sample)
- [ ] Have read [Codecademy BI Fundamentals](https://www.codecademy.com/learn/bi-fundamentals) or equivalent
- [ ] Familiar with basic BI terms: KPI, dimension, measure, fact table, DAX
- [ ] Understand SQL basics (SELECT, WHERE, JOIN) - recommended but not required

**Not ready?** Review [Microsoft Learn - Power BI Getting Started](https://learn.microsoft.com/en-us/training/modules/get-started-with-power-bi/) before proceeding.

---

## 🔧 Environment Setup

### Step 1: Create Project Folder Structure
```
learn_bi_project/
├── data/
│   ├── accounts.csv
│   ├── transactions.csv
│   └── ...
├── reports/
│   └── financial_dashboard.pbix
└── documentation/
    ├── requirements.md
    ├── data_model.md
    └── dashboard_spec.md
```

### Step 2: Configure Power BI Desktop
1. **Open Power BI Desktop**
2. **File → Options and Settings → Options**
3. **Under "Global":**
   - Set "Date/Time Format" to your locale
   - Enable "Preview features" (optional, for latest features)
4. Click **OK**

### Step 3: Create First Power BI File
1. **File → New**
2. **Save As** → `financial_dashboard.pbix` in your project folder
3. **Leave blank for now** (we'll populate in Quick Start)

---

## 🐛 Troubleshooting

### "Power BI Desktop won't start"
- **Solution:** Uninstall via Settings → Apps, then reinstall from Microsoft Store
- **Reference:** [Microsoft - Power BI Installation Issues](https://learn.microsoft.com/en-us/power-bi/fundamentals/power-bi-service-overview)

### "I can't find the Power Query Editor"
- **Location:** Home tab → Transform Data (top-left)
- **Keyboard shortcut:** Ctrl + Shift + X
- **Reference:** [Microsoft Learn - Power Query](https://learn.microsoft.com/en-us/training/modules/work-with-power-query-in-power-bi-desktop/)

### "Permission denied when accessing CSV files"
- **Solution:** Close Excel if file is open; files can't be read while locked
- **Alternative:** Copy CSV to new location before importing

### "Data source not found after restart"
- **Cause:** Relative file paths changed
- **Solution:** Use absolute paths or store data in project folder (see Step 1)

---

## 📖 Next Steps

✅ All prerequisites met? Proceed to [QUICK_START.md](QUICK_START.md) to build your first dashboard in 60 minutes.

❓ Questions? Review the [REFERENCE_MATERIALS.md](REFERENCE_MATERIALS.md) glossary for term definitions.

---

## 📚 Recommended Review (30 minutes)

If any of these are unfamiliar, spend 30 minutes reviewing:

1. **BI Fundamentals** (if not completed via Codecademy)
   - Microsoft Learn: https://learn.microsoft.com/en-us/training/paths/bi-analyst/

2. **Power BI Overview** (15 min)
   - Microsoft Learn: https://learn.microsoft.com/en-us/training/modules/get-started-with-power-bi/

3. **DAX Basics** (15 min)
   - Microsoft Learn: https://learn.microsoft.com/en-us/training/modules/understand-dax-fundamentals/

---

**Ready?** Start with [QUICK_START.md](QUICK_START.md) →
