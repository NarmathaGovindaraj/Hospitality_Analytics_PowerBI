# Support Runbook
## Hospitality Analytics Dashboard — Incident Response & Operations

---

## 1. Incident Classification

| Priority | Definition | Response SLA | Resolution SLA | Examples |
|----------|-----------|-------------|----------------|----------|
| P1 — Critical | Data security breach or complete outage | 15 minutes | 2 hours | RLS failure, all pages broken, data leakage |
| P2 — High | Major functionality broken | 1 hour | 8 hours | Key KPI wrong, refresh failed >24h, page not loading |
| P3 — Medium | Minor functionality issue | 4 hours | 3 days | Single visual broken, formatting error, tooltip missing |
| P4 — Low | Cosmetic or enhancement | 1 day | Next sprint | Color preference, label alignment, new feature request |

---

## 2. Common Issues & Resolution Playbooks

### 2.1 PLAYBOOK: Dashboard Shows Blank Data

**Symptoms:** User sees blank page, no KPI values, empty charts

**Triage Steps:**
1. Ask user: Which page? All pages or specific?
2. Ask user: What is your email address?
3. Check: Is user assigned to workspace? (Workspace → Access)
4. Check: Is user assigned to correct RLS role? (Dataset → Security)

**Resolution Tree:**
```
Is user a Manager?
├── YES → Check RLS assignment
│   ├── Email in HotelManager role? → If NO, add them
│   ├── Email matches dim_Manager exactly? → If NO, fix case/spelling
│   └── Relationship R3 bidirectional? → If NO, fix in Desktop + republish
└── NO (Executive/Viewer) → Should see all data
    ├── User accidentally in HotelManager role? → Remove from role
    └── Dataset credentials expired? → Re-authenticate, refresh
```

### 2.2 PLAYBOOK: Scheduled Refresh Failed

**Symptoms:** Refresh history shows "Failed" status, data is stale

**Triage Steps:**
1. Dataset → Refresh history → Note error message
2. Identify error category:

| Error Contains | Root Cause | Fix |
|---------------|-----------|-----|
| "credentials" | Token expired | Dataset Settings → Edit credentials → Re-sign in |
| "file not found" | Source file moved | Update path in PQ or restore file to expected location |
| "gateway" | Gateway offline | Restart gateway service (see Section 2.8) |
| "timeout" | File locked or too large | Check if file is open, wait, retry |
| "memory" | Dataset too large | Optimize model, reduce columns |
| "encoding" | File saved incorrectly | Verify OnlineBooking uses Latin-1 encoding |

**Resolution:**
1. Fix root cause per table above
2. Trigger manual refresh: Dataset → ⋯ → Refresh now
3. Verify success in Refresh history
4. Monitor next scheduled refresh
5. Close incident

### 2.3 PLAYBOOK: RLS Not Filtering Correctly

**Symptoms:** Manager sees other hotels' data, or executive sees filtered data

**Triage Steps:**
1. Identify: Who is affected? What do they see?
2. Emergency: If data leakage → IMMEDIATELY remove all users from role

**Resolution (Manager sees too much data):**
1. Verify dim_Manager table has correct email-to-hotel mapping
2. Verify relationship R3 is bidirectional (Both) in Desktop
3. Verify role filter: `[ManagerEmail] = USERPRINCIPALNAME()`
4. Verify email is lowercase in dim_Manager
5. Test in Service: Dataset → Security → Test as role
6. If fixed: Republish and re-add users

**Resolution (Executive sees filtered data):**
1. Check: Is executive accidentally assigned to HotelManager role?
2. Remove them from role (executives should be in NO role)
3. Verify they now see all data

### 2.4 PLAYBOOK: KPI Value Seems Wrong

**Symptoms:** User reports revenue or other metric doesn't match expectations

**Triage Steps:**
1. Ask: Which KPI? Which page? What filters are active?
2. Ask user to clear all slicers and check again
3. Compare reported value to Reference Values (Testing doc Appendix A)

**Resolution Tree:**
```
Value matches reference values?
├── YES → User had filters active (explain slicer state)
│   └── Train user on "Reset All" bookmark
└── NO → Possible DAX error
    ├── Check measure formula in Desktop
    ├── Verify PaymentStatus filter is applied
    ├── Check if recent data load introduced bad data
    └── Fix formula → Republish → Verify
```

### 2.5 PLAYBOOK: Page Not Loading / Visual Error

**Symptoms:** Specific page shows error banner, visual displays "Can't display"

**Triage Steps:**
1. Which page? Which visual?
2. Can other users access the same page?
3. Try in Incognito/private browser

**Resolution:**
1. If all users affected:
   - Open .pbix in Desktop → check for errors
   - Possible cause: Source column removed or renamed
   - Fix in Power Query → republish
2. If single user affected:
   - Clear browser cache
   - Try different browser
   - Check Pro license is active

### 2.6 PLAYBOOK: Slicer Not Working

**Symptoms:** Clicking slicer doesn't change visuals

**Triage Steps:**
1. Which slicer? Which page?
2. Is it a sync issue (works on one page, not another)?

**Resolution:**
1. In Desktop → View → Sync slicers → verify sync configuration
2. Check Edit Interactions: Is the visual set to "None" for that slicer?
3. Check relationships: Is the slicer field connected to the visual's table?
4. Fix → republish

### 2.7 PLAYBOOK: Drill-Through Not Available

**Symptoms:** Right-click shows no "Drill through" option

**Triage Steps:**
1. Confirm user is right-clicking on a hotel name field
2. Confirm drill-through page exists and has filter field configured

**Resolution:**
1. Open Desktop → Navigate to drill-through page
2. Verify: "Add drill-through fields here" has dim_Hotel[Hotel_Name]
3. Verify page is not accidentally deleted
4. Republish

### 2.8 PLAYBOOK: Gateway Offline

**Symptoms:** Refresh fails with gateway error, gateway shows "Offline"

**Resolution:**
1. RDP to gateway server: `[gateway-server-address]`
2. Open Services (services.msc)
3. Find: "On-premises data gateway service"
4. Status should be: Running
5. If Stopped: Right-click → Start
6. Wait 2 minutes
7. Check Power BI Service → Manage gateways → Status should be "Online"
8. If still offline:
   - Check internet connectivity on gateway server
   - Check firewall rules (outbound 443 to *.servicebus.windows.net)
   - Check Windows event logs for errors
   - Escalate to L3 (IT Infrastructure)

---

## 3. Monitoring Dashboard

### 3.1 Daily Health Check (2 minutes)

| # | Check | How | Expected |
|---|-------|-----|----------|
| 1 | Refresh succeeded | Dataset → Refresh history | "Completed" today |
| 2 | No alert emails | Email inbox | No failure notifications |
| 3 | Report loads | Open app → Page 1 | KPI cards populated |

### 3.2 Weekly Health Check (15 minutes)

| # | Check | How | Expected |
|---|-------|-----|----------|
| 1 | All daily checks pass | See above | ✓ |
| 2 | Usage metrics | Workspace → Usage metrics | Active users > 0 |
| 3 | RLS spot check | Test as 1 random manager | Correct filtering |
| 4 | Revenue sanity check | Compare to prior week | No unexpected jumps/drops |
| 5 | Gateway status (if applicable) | Manage gateways | Online |

---

## 4. Escalation Matrix

```
User reports issue
    │
    ▼
L1: BI Admin
    ├── Can resolve (credential, access, slicer) → Resolve & close
    └── Cannot resolve
        │
        ▼
    L2: BI Lead / Developer
        ├── Can resolve (DAX fix, PQ fix, republish) → Resolve & close
        └── Cannot resolve (infrastructure)
            │
            ▼
        L3: IT Infrastructure
            ├── Gateway, network, Azure AD → Resolve & close
            └── Unresolvable
                │
                ▼
            L4: Project Owner / Vendor escalation
```

---

## 5. Communication Templates

### 5.1 Refresh Failure Notification (to stakeholders)

```
Subject: Hospitality Analytics — Data Refresh Delayed

Team,

The scheduled data refresh for the Hospitality Analytics dashboard
did not complete successfully this morning.

Impact: Data shown is from [last successful refresh date].
Cause: [brief description]
Status: Under investigation
ETA: [expected resolution time]

The dashboard remains accessible with slightly stale data.
No action needed from your side.

— BI Team
```

### 5.2 Incident Resolved Notification

```
Subject: ✅ RESOLVED — Hospitality Analytics Dashboard

Team,

The issue reported on [date] has been resolved.

Root Cause: [brief description]
Resolution: [what was done]
Preventive Action: [what will prevent recurrence]

The dashboard is now fully operational with current data.

— BI Team
```

### 5.3 Planned Maintenance Notification

```
Subject: Planned Maintenance — Hospitality Analytics (Saturday [date])

Team,

We will be performing scheduled maintenance on the Hospitality Analytics
dashboard this Saturday from 10:00 AM to 12:00 PM IST.

During this window:
- Dashboard may be temporarily unavailable
- Data refresh will be paused

No action required. Service will resume automatically after maintenance.

— BI Team
```

---

## 6. Knowledge Base (FAQ)

| # | Question | Answer |
|---|----------|--------|
| 1 | How often is data refreshed? | Daily at 6:00 AM IST |
| 2 | Why does the manager see blank data? | Their email is not in RLS role or doesn't match dim_Manager |
| 3 | Can users export to Excel? | Yes, if they have Viewer+ access. Exports respect RLS. |
| 4 | Can we add a new hotel? | Yes — add row to Hotel_List + Manager_Details, update fact data, republish |
| 5 | Can we change the refresh time? | Yes — Dataset Settings → Scheduled refresh → change time |
| 6 | Where is the .pbix backup? | Git repository: hospitality-analytics/src/ |
| 7 | What Power BI license is needed? | Pro (for all users) or Premium Per User |
| 8 | Who can modify the report? | Only Admin/Member roles in workspace |
| 9 | Is mobile access supported? | Basic functionality via Power BI mobile app |
| 10 | How do we add a new KPI? | Change request → develop in Dev workspace → test → deploy |

---

## 7. Disaster Recovery Plan

| Scenario | RTO | RPO | Procedure |
|----------|-----|-----|-----------|
| Report deleted | 1 hour | 0 (no data loss) | Republish from .pbix backup |
| Dataset corrupted | 2 hours | 24 hours (last refresh) | Republish, reconfigure refresh + RLS |
| Workspace deleted | 4 hours | 0 | Restore (Premium) or rebuild from backup |
| Source files lost | 4 hours | 24 hours | Restore from file backup (90-day retention) |
| Gateway server failure | 8 hours | Until new gateway | Install gateway on standby server |
| Azure AD outage | N/A | N/A | Wait for Microsoft resolution (out of our control) |

**Recovery Order:**
1. Restore .pbix → Publish to workspace
2. Configure data source credentials
3. Set up scheduled refresh
4. Assign RLS roles (7 managers)
5. Test (3 managers + 1 executive)
6. Notify users of restoration
