# Deployment Guide
## Hospitality Analytics Dashboard — Production Deployment

| Field | Value |
|-------|-------|
| Phase | Phase 5 — Deployment |
| Version | 1.0 |
| Date | June 2026 |
| Environment | Power BI Service (app.powerbi.com) |
| Artifact | Hospitality_Analytics_Dashboard.pbix |

---

## 1. Pre-Deployment Checklist

| # | Item | Status | Notes |
|---|------|--------|-------|
| 1 | All Phase 4 tests passed (0 Critical/High defects) | ☐ | |
| 2 | Final sign-off obtained from Project Owner | ☐ | |
| 3 | .pbix file saved and named correctly | ☐ | Hospitality_Analytics_Dashboard.pbix |
| 4 | Power BI Pro licenses confirmed for all users | ☐ | 10+ users |
| 5 | Azure AD accounts exist for all managers | ☐ | 7 manager emails |
| 6 | Workspace admin access available | ☐ | Deployer has Admin role |
| 7 | Data source files accessible from refresh location | ☐ | Shared drive or SharePoint |
| 8 | Gateway installed (if files on on-premises network) | ☐ | See Section 5 |
| 9 | Backup of .pbix stored in version control | ☐ | See Section 11 |
| 10 | Stakeholder communication sent (go-live date) | ☐ | |

---

## 2. Workspace Creation & Permissions

### 2.1 Create Workspace

1. Go to **app.powerbi.com**
2. Click **Workspaces** → **Create a workspace**
3. Configure:
   - Name: `Hospitality Analytics - Production`
   - Description: "Hospitality performance dashboard for Taj hotel properties"
   - Advanced:
     - License mode: **Pro** (or Premium Per User if available)
     - Contact list: BI Lead email
4. Click **Save**

### 2.2 Assign Workspace Roles

| User / Group | Role | Access Level | Justification |
|-------------|------|--------------|---------------|
| BI Lead | Admin | Full control | Manages workspace, datasets, reports |
| Project Owner | Admin | Full control | Business ownership |
| Hospitality Head | Member | Edit + share | Strategic oversight |
| Finance Team (group) | Viewer | View only | Consumes dashboards |
| Guest Experience (group) | Viewer | View only | Consumes dashboards |
| Marketing Team (group) | Viewer | View only | Consumes dashboards |
| Hotel Managers (7 users) | Viewer | View only (RLS filtered) | Property-specific access |

### 2.3 Steps to Add Members

1. In workspace → **Access** (top bar)
2. Enter email/group → Select role → **Add**
3. Repeat for each user/group

---

## 3. Publish Report

### 3.1 Publish from Desktop

1. Open `Hospitality_Analytics_Dashboard.pbix` in Power BI Desktop
2. Verify you're signed in (top-right corner)
3. Home → **Publish**
4. Select workspace: `Hospitality Analytics - Production`
5. If prompted about existing report, choose **Replace**
6. Wait for "Success" confirmation
7. Click "Open 'Hospitality_Analytics_Dashboard' in Power BI"

### 3.2 Verify in Service

1. Navigate to workspace in browser
2. Confirm both objects exist:
   - **Report:** Hospitality_Analytics_Dashboard
   - **Dataset:** Hospitality_Analytics_Dashboard
3. Open report → verify all 6 pages render correctly
4. Check data appears (not blank)

---

## 4. Row-Level Security Assignment in Service

### 4.1 Navigate to Security Settings

1. In workspace, find the **Dataset** (not report)
2. Click **⋯** (more options) → **Security**

### 4.2 Assign Users to HotelManager Role

1. Select the **HotelManager** role (left panel)
2. Add members one at a time:

| # | Email to Add | Click Add |
|---|-------------|-----------|
| 1 | aman.mehta@tajhotels.com | ✓ |
| 2 | kavya.shah@tajhotels.com | ✓ |
| 3 | rohan.das@tajhotels.com | ✓ |
| 4 | sneha.roy@tajhotels.com | ✓ |
| 5 | arjun.rao@tajhotels.com | ✓ |
| 6 | priya.nair@tajhotels.com | ✓ |
| 7 | sanjay.iyer@tajhotels.com | ✓ |

3. **DO NOT** add executives or leadership to any role

### 4.3 Verify RLS in Service

1. On Security page, click **⋯** next to HotelManager → **Test as role**
2. Enter `aman.mehta@tajhotels.com` in "Other user" field
3. Click **Run** → Verify only Taj Mahal Palace data appears
4. Repeat for at least 2 other managers
5. Test with no role selected → verify all data visible

---

## 5. Gateway Configuration

### 5.1 When is a Gateway Needed?

| Data Source Location | Gateway Required? |
|---------------------|-------------------|
| Local files (C:\) | Yes — On-premises data gateway |
| Network share (\\server\) | Yes — On-premises data gateway |
| SharePoint Online | No — cloud connector |
| OneDrive for Business | No — cloud connector |
| Azure Blob Storage | No — cloud connector |
| SQL Server (on-prem) | Yes — On-premises data gateway |

### 5.2 Gateway Installation (If Required)

1. Download from: https://powerbi.microsoft.com/gateway/
2. Install on a server with:
   - Always-on availability
   - Network access to data source files
   - Minimum 8 GB RAM, 2 CPU cores
3. Register gateway with Power BI Service:
   - Sign in with admin account during setup
   - Name: `Hospitality-Gateway-Prod`
   - Recovery key: [store securely]

### 5.3 Configure Data Source on Gateway

1. Power BI Service → Settings → **Manage gateways**
2. Select gateway → **Add data source**
3. Configure:
   - Name: `Hospitality_CSV_Source`
   - Type: File
   - Path: Network path to CSV/Excel folder
4. Map dataset to gateway:
   - Dataset Settings → Gateway connection → Select gateway → Map sources

### 5.4 If Using SharePoint (No Gateway)

1. Upload all 6 data files to SharePoint document library
2. In Power BI Desktop, change source paths to SharePoint URLs
3. Republish
4. Dataset Settings → Data source credentials → Sign in with SharePoint access

---

## 6. Scheduled Refresh Configuration

### 6.1 Configure Refresh

1. Workspace → Dataset → **⋯** → **Settings**
2. Expand **Scheduled refresh**
3. Toggle: **On**
4. Configure:

| Setting | Value |
|---------|-------|
| Refresh frequency | Daily |
| Time zone | (UTC+05:30) Chennai, Kolkata, Mumbai, New Delhi |
| Time slots | 06:00 AM (before business hours) |
| Additional slot | 12:00 PM (optional midday refresh) |
| Send refresh failure notifications | ☑ To dataset owner |
| Additional notification email | bi.team@tajhotels.com |

5. Click **Apply**

### 6.2 Verify Credentials

1. In Dataset Settings → **Data source credentials**
2. For each source, click **Edit credentials**
3. Authentication method: **Organizational account** (for SharePoint) or **Windows** (for gateway)
4. Privacy level: **Organizational**
5. Click **Sign in**

### 6.3 Manual Refresh Test

1. Dataset → **⋯** → **Refresh now**
2. Wait for completion (check refresh history)
3. Verify: Dataset → **⋯** → **Refresh history**
4. Confirm status: "Completed" with timestamp
5. Open report → verify data is current

---

## 7. App Publishing

### 7.1 Create App

1. In workspace → **Create app** (top bar)
2. Configure **Setup** tab:
   - App name: `Hospitality Analytics`
   - Description: "Performance analytics for Taj hotel properties"
   - App logo: Upload Taj logo (optional)
   - Contact information: BI Lead email
   - Support site: Internal wiki link

3. Configure **Navigation** tab:
   - Include pages:
     - ☑ Executive Summary
     - ☑ Revenue & Trends
     - ☑ Hotel Manager View
     - ☑ Channel Analysis
     - ☑ Guest Satisfaction
     - ☑ Member vs Non-Member
   - Exclude hidden pages:
     - ☐ Hotel Drillthrough (hidden — accessed via drill-through only)

4. Configure **Permissions** tab:
   - Access: **Specific users and groups**
   - Add:
     - Hospitality Leadership (security group)
     - Hotel Managers (security group)
     - Finance Team (security group)
     - Marketing Team (security group)
     - Guest Experience Team (security group)
   - Allow users to: ☑ Share the app

5. Click **Publish app**

### 7.2 Distribute App Link

Send to all users:
```
App URL: https://app.powerbi.com/Redirect?action=InstallApp&appId=[APP_ID]
```

Or instruct users:
1. Open Power BI Service → **Apps** (left nav)
2. Click **Get apps** → Search "Hospitality Analytics"
3. Click **Get it now**

### 7.3 Update App (Future Releases)

1. Make changes to report in workspace
2. Go to workspace → **Update app**
3. Review changes → **Update app**
4. Users see updates automatically (no reinstall needed)

---

## 8. Rollback Strategy

### 8.1 Rollback Triggers

| Severity | Trigger | Action |
|----------|---------|--------|
| Critical | RLS failure (data leakage) | Immediate rollback + access revoke |
| Critical | Wrong revenue figures affecting decisions | Immediate rollback |
| High | Multiple pages broken | Rollback within 4 hours |
| High | Refresh failure persisting > 24h | Investigate, rollback if needed |
| Medium | Single visual incorrect | Fix forward (no rollback) |

### 8.2 Rollback Procedure

**Option A: Republish Previous Version**
1. Retrieve previous .pbix from version control (Git/SharePoint)
2. Open in Power BI Desktop
3. Home → Publish → Select same workspace → Replace
4. Verify report renders correctly
5. Re-test RLS (critical)
6. Notify users of temporary rollback

**Option B: Restore from Power BI Service Backup (Premium only)**
1. Admin portal → Workspace → Restore
2. Select restore point (if available)
3. Verify data and visuals

**Option C: Emergency Access Revoke**
1. If data leakage suspected:
   - Dataset → Security → Remove ALL members from HotelManager role
   - This blocks filtered access immediately
   - Executives retain full access (no role = all data)
2. Investigate root cause
3. Fix and re-assign after verification

### 8.3 Rollback Communication Template

```
Subject: [URGENT] Hospitality Analytics Dashboard — Temporary Rollback

Team,

We have identified an issue with the Hospitality Analytics dashboard
and have temporarily reverted to the previous version while we
investigate.

Impact: [describe what was incorrect]
Status: Under investigation
ETA for fix: [timeline]

The dashboard remains available with the prior version's data.
No action required from your side.

— BI Team
```

---

## 9. Production Deployment Checklist

| # | Step | Action | Responsible | Status |
|---|------|--------|-------------|--------|
| 1 | Create workspace | Section 2 | BI Admin | ☐ |
| 2 | Assign workspace roles | Section 2.2 | BI Admin | ☐ |
| 3 | Publish .pbix | Section 3 | BI Developer | ☐ |
| 4 | Configure RLS in Service | Section 4 | BI Admin | ☐ |
| 5 | Test RLS (all 7 managers) | Section 4.3 | QA | ☐ |
| 6 | Install/configure gateway (if needed) | Section 5 | IT Admin | ☐ |
| 7 | Configure data source credentials | Section 6.2 | BI Admin | ☐ |
| 8 | Set up scheduled refresh | Section 6.1 | BI Admin | ☐ |
| 9 | Run manual refresh test | Section 6.3 | BI Admin | ☐ |
| 10 | Create and publish App | Section 7 | BI Admin | ☐ |
| 11 | Distribute app link to users | Section 7.2 | Project Owner | ☐ |
| 12 | Verify end-user access (sample user) | Login as viewer | QA | ☐ |
| 13 | Verify RLS from end-user perspective | Login as manager | QA | ☐ |
| 14 | Store .pbix in version control | Section 11 | BI Developer | ☐ |
| 15 | Send go-live notification | Email template | Project Owner | ☐ |

---

## 10. Post-Deployment Validation

| # | Validation | Method | Expected | Pass/Fail |
|---|-----------|--------|----------|-----------|
| 1 | Report loads in Service | Open app link | All 6 pages render | |
| 2 | Data is current | Check Last Refresh timestamp | Today's date | |
| 3 | KPI cards show values | Visual inspection | Non-zero, formatted correctly | |
| 4 | Slicers work | Click Year slicer | Data filters correctly | |
| 5 | RLS filters correctly | Login as manager | Only their hotel visible | |
| 6 | Drill-through works | Right-click hotel bar | Navigates to detail page | |
| 7 | Export to Excel works | Export a visual | Data downloads correctly | |
| 8 | Mobile layout accessible | Open on phone browser | Basic functionality works | |
| 9 | Scheduled refresh succeeds | Check refresh history next day | Status: Completed | |
| 10 | App accessible to all users | Confirm with 3 sample users | Can open and interact | |

---

## 11. Version Control Recommendations

### 11.1 Repository Structure

```
hospitality-analytics/
├── README.md
├── src/
│   ├── Hospitality_Analytics_Dashboard.pbix    (current version)
│   └── Hospitality_Theme.json                  (custom theme file)
├── data/
│   ├── Hospitality_Data_Global_fct.csv
│   ├── Finance_dim.csv
│   ├── Feedback_dim.csv
│   ├── OnlineBooking_dim.csv
│   ├── Hotel_List_dim.xlsx
│   └── Manager_Details_dim.xlsx
├── docs/
│   ├── Business_Requirements_Document.md
│   ├── Functional_Specification.md
│   ├── Star_Schema_Design.md
│   ├── Power_Query_Transformations.md
│   ├── DAX_Measures.md
│   ├── Dashboard_Page_Design.md
│   ├── RLS_Implementation.md
│   ├── Testing_Checklist.md
│   ├── Deployment_Guide.md
│   ├── User_Manual.md
│   └── Administrator_Guide.md
├── archive/
│   └── v1.0/
│       └── Hospitality_Analytics_Dashboard_v1.0.pbix
└── CHANGELOG.md
```

### 11.2 Versioning Strategy

| Version | Trigger | Action |
|---------|---------|--------|
| v1.0 | Initial production release | Tag + archive .pbix |
| v1.1 | Bug fixes (DAX corrections, visual fixes) | Increment minor |
| v1.2 | New slicer or visual added | Increment minor |
| v2.0 | New report page or major redesign | Increment major |

### 11.3 Change Log Template (CHANGELOG.md)

```markdown
## [1.0.0] - 2026-06-18
### Added
- Initial 6-page dashboard deployment
- 51 DAX measures
- RLS for 7 hotel managers
- Scheduled daily refresh at 06:00 AM IST
```

### 11.4 .gitignore for Power BI Projects

```
# Power BI
*.pbit
*.pbids
.pbi/

# OS
.DS_Store
Thumbs.db

# Temp
~$*
*.tmp
```

---

## 12. Monitoring & Maintenance

### 12.1 Daily Monitoring

| Task | Method | Frequency | Owner |
|------|--------|-----------|-------|
| Check refresh status | Dataset → Refresh history | Daily (AM) | BI Admin |
| Review refresh failure alerts | Email notifications | On-trigger | BI Admin |
| Spot-check KPI accuracy | Compare card to source | Weekly | Data Analyst |

### 12.2 Weekly Maintenance

| Task | Method | Frequency | Owner |
|------|--------|-----------|-------|
| Review usage metrics | Workspace → Usage metrics report | Weekly | BI Lead |
| Check user access requests | Email inbox | Weekly | BI Admin |
| Verify RLS still works correctly | Test as 1 random manager | Weekly | QA |

### 12.3 Monthly Maintenance

| Task | Method | Frequency | Owner |
|------|--------|-----------|-------|
| Review and archive old data (if needed) | Power Query | Monthly | Data Analyst |
| Check for Power BI Desktop updates | Microsoft download page | Monthly | BI Developer |
| Review and rotate gateway credentials | Gateway admin | Monthly | IT Admin |
| Backup .pbix to version control | Git commit | Monthly (min) | BI Developer |
| Review user access list (remove departed) | Workspace → Access | Monthly | BI Admin |

### 12.4 Quarterly Review

| Task | Owner |
|------|-------|
| Stakeholder feedback collection | Project Owner |
| KPI relevance review (add/retire metrics) | Business Analyst |
| Performance review (load times trending) | BI Developer |
| Security audit (RLS + access) | IT Security |

### 12.5 Alerting Setup

Configure in Power BI Service:
1. For each KPI card on Executive Summary:
   - Pin to dashboard
   - Set alert: "Notify me when Total Revenue falls below ₹X"
   - Set alert: "Notify me when GSS drops below 3.5"
2. Dataset refresh failure → auto-email to BI Admin

---

## 13. Backup & Recovery

### 13.1 Backup Strategy

| What | Where | Frequency | Retention |
|------|-------|-----------|-----------|
| .pbix file | Git repository + SharePoint | Every change | All versions |
| Source data files | Network share + cloud backup | Daily | 90 days |
| Power BI workspace | Power BI Service (built-in) | Continuous | Per license |
| Gateway config | Documented in runbook | On change | Current + 1 prior |

### 13.2 Recovery Procedures

**Scenario A: Report corrupted**
1. Retrieve last known good .pbix from Git/SharePoint
2. Open in Desktop → verify → Publish (replace)
3. Re-verify RLS assignments

**Scenario B: Dataset deleted**
1. Republish from .pbix file
2. Reconfigure: credentials, refresh schedule, RLS assignments
3. App will automatically reconnect

**Scenario C: Workspace deleted (Premium only)**
1. Admin portal → Deleted workspaces → Restore
2. Verify all artifacts restored

**Scenario D: Source data corruption**
1. Identify corrupt file via refresh error message
2. Replace with backup copy from 90-day retention
3. Trigger manual refresh
4. Validate KPIs against reference values (Testing doc Appendix A)

---

## 14. Go-Live Communication

### 14.1 Go-Live Email Template

```
Subject: 🎉 Hospitality Analytics Dashboard — Now Live!

Dear Team,

We are excited to announce that the Hospitality Analytics Dashboard
is now live and available for your use.

ACCESS:
• App Link: [INSERT LINK]
• Or: Power BI Service → Apps → "Hospitality Analytics"

WHAT YOU'LL FIND:
• Executive Summary — Portfolio-wide KPIs and trends
• Revenue & Trends — Multi-year financial analysis
• Hotel Manager View — Your property's performance (RLS-secured)
• Channel Analysis — Booking source insights
• Guest Satisfaction — Ratings and feedback
• Member vs Non-Member — Loyalty program analysis

DATA REFRESH:
• Automatically refreshed daily at 6:00 AM IST
• Data covers: January 2021 – September 2025

HOTEL MANAGERS:
You will only see data for your assigned property. This is by design
(Row-Level Security). If you see blank data, please contact the BI team.

SUPPORT:
For issues or questions, contact: bi.team@tajhotels.com

Best regards,
[Project Owner Name]
```
