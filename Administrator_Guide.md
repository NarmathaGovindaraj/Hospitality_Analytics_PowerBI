# Administrator Guide
## Hospitality Analytics Dashboard — Admin Runbook

---

## 1. System Overview

| Component | Details |
|-----------|---------|
| Report Name | Hospitality_Analytics_Dashboard |
| Workspace | Hospitality Analytics - Production |
| Dataset Size | ~100,000 rows across 7 tables |
| File Size | ~20 MB (.pbix) |
| Refresh | Daily at 06:00 AM IST |
| RLS | 1 dynamic role (HotelManager) with 7 assigned users |
| Users | ~15 total (7 managers + executives + teams) |
| Gateway | Required if source files on-premises |

---

## 2. Admin Tasks — Quick Reference

### 2.1 Add a New User (Viewer)

1. Workspace → **Access** → Enter email → Role: **Viewer** → Add
2. If they need the App: App → Update → Permissions → Add user → Update app

### 2.2 Add a New Hotel Manager (with RLS)

1. First: Add their row to `Manager_Details_dim.xlsx`:
   - HotelName, ManagerID, ManagerName, ManagerEmail
2. Republish .pbix (so dim_Manager table includes new row)
3. Workspace → Dataset → Security → HotelManager role → Add member email
4. Test: Security → Test as role → verify correct filtering
5. Add to workspace as Viewer
6. Add to App permissions

### 2.3 Remove a User

1. Workspace → Access → Find user → Remove
2. If manager: Dataset → Security → HotelManager → Remove member
3. Update App permissions (Update app → Permissions → remove)

### 2.4 Change a Hotel Manager

If MGR001 is replaced by a new manager:
1. Update `Manager_Details_dim.xlsx` with new name + email
2. Republish .pbix
3. Dataset → Security → HotelManager role:
   - Remove old email
   - Add new email
4. Workspace → Access: Remove old user, add new user as Viewer
5. Test RLS with new email

### 2.5 Trigger Manual Refresh

1. Workspace → Dataset → **⋯** → **Refresh now**
2. Monitor: Dataset → **⋯** → **Refresh history**
3. Expected duration: < 60 seconds

### 2.6 Change Refresh Schedule

1. Dataset → **⋯** → **Settings**
2. Scheduled refresh → Modify time/frequency
3. Click **Apply**

### 2.7 Update Data Source Credentials

1. Dataset → Settings → **Data source credentials**
2. Click **Edit credentials** for each source
3. Re-authenticate
4. Test with manual refresh

---

## 3. Refresh Failure Troubleshooting

### 3.1 Common Failure Causes

| Error | Cause | Solution |
|-------|-------|----------|
| "Data source error" | Credential expired | Dataset Settings → Edit credentials → Re-sign in |
| "File not found" | Source file moved/renamed | Update file path in Power Query, republish |
| "Gateway unreachable" | Gateway service stopped | Restart gateway service on server |
| "Timeout" | Network/file lock | Check if file is open by another process |
| "Access denied" | Permission change on source | Verify service account has read access |
| "Encoding error" | File saved in wrong encoding | Ensure OnlineBooking uses Latin-1 (1252) |

### 3.2 Refresh Failure Recovery Steps

1. Check **Refresh history** for error details
2. Identify root cause from error message
3. Fix the issue (credentials, path, gateway, etc.)
4. Trigger manual refresh to verify fix
5. Confirm scheduled refresh succeeds next day
6. If > 24h failure: notify stakeholders

### 3.3 Gateway Health Check

1. Go to **Settings** → **Manage gateways**
2. Verify status: **Online** (green)
3. If offline:
   - RDP to gateway server
   - Services → "On-premises data gateway service" → Restart
   - Wait 2 minutes → check status in portal
4. If still offline: Check network connectivity, firewall rules

---

## 4. Security Administration

### 4.1 RLS Role Summary

| Role | Filter | Table | Users Assigned |
|------|--------|-------|----------------|
| HotelManager | [ManagerEmail] = USERPRINCIPALNAME() | dim_Manager | 7 managers |

### 4.2 Monthly Security Audit

Checklist:
- [ ] Verify all 7 managers still employed
- [ ] Remove departed employees from role + workspace
- [ ] Test RLS for at least 2 random managers
- [ ] Verify no executive is accidentally assigned to HotelManager role
- [ ] Check workspace access list for unauthorized users
- [ ] Review export permissions (are viewers exporting sensitive data?)

### 4.3 Emergency Security Response

**If data leakage suspected (manager sees other hotels):**
1. IMMEDIATELY: Dataset → Security → Remove ALL members from role
2. Investigate: Check dim_Manager data, relationship R3, cross-filter direction
3. Fix root cause
4. Re-test with View As
5. Re-add members one by one, testing each
6. Document incident

---

## 5. Capacity & Performance Management

### 5.1 Performance Baseline

| Metric | Baseline | Alert Threshold |
|--------|----------|-----------------|
| Page load time | < 3 seconds | > 5 seconds |
| Refresh duration | < 45 seconds | > 120 seconds |
| .pbix file size | ~20 MB | > 50 MB |
| Dataset memory | ~80 MB | > 200 MB |
| DAX query time (complex) | < 2 seconds | > 5 seconds |

### 5.2 Performance Degradation Response

If performance degrades:
1. Check if data volume has grown significantly
2. Review recent changes (new measures? new visuals?)
3. Run Performance Analyzer in Desktop:
   - View → Performance analyzer → Start recording
   - Navigate pages → check which visuals are slow
4. Optimize slowest visuals:
   - Reduce visual count per page
   - Simplify DAX (use variables, pre-calculate)
   - Check for unnecessary bidirectional relationships

### 5.3 Capacity Planning

| Trigger | Action |
|---------|--------|
| Data grows past 500K rows | Consider aggregation tables or incremental refresh |
| Users exceed 50 | Consider Premium Per User or Premium capacity |
| Refresh window exceeds 30 minutes | Optimize Power Query, consider incremental refresh |
| Multiple concurrent users slow reports | Consider Premium capacity with load balancing |

---

## 6. Update & Change Management

### 6.1 Change Request Process

1. User submits request (new visual, new KPI, bug fix)
2. BI Lead assesses impact (Low/Medium/High)
3. Changes made in Development workspace (copy of prod)
4. Testing performed (follow Testing_Checklist.md)
5. Approval from stakeholder
6. Deploy to Production workspace (republish)
7. Update version number and CHANGELOG

### 6.2 Development vs Production Separation

| Workspace | Purpose | Who Has Access |
|-----------|---------|----------------|
| Hospitality Analytics - Development | Building and testing changes | BI Team only |
| Hospitality Analytics - Production | Live user access | All stakeholders |

**Never make changes directly in Production.** Always develop → test → promote.

### 6.3 Deployment Procedure (Updates)

1. Verify changes in Development workspace
2. Save updated .pbix
3. In Production workspace: Publish → Replace existing
4. Verify: Open report, check all pages
5. Verify: RLS still works (Test as role)
6. Verify: Scheduled refresh still configured
7. Update CHANGELOG.md
8. Commit .pbix to version control
9. Notify users if significant changes

---

## 7. Contact & Escalation

| Level | Contact | Response Time | Trigger |
|-------|---------|---------------|---------|
| L1 | BI Admin (bi.team@tajhotels.com) | < 4 hours | Any issue |
| L2 | BI Lead | < 8 hours | L1 cannot resolve |
| L3 | IT Infrastructure | < 24 hours | Gateway/network/Azure AD issues |
| Executive | Project Owner | Immediate | Data leakage, critical business impact |

---

## 8. Scheduled Maintenance Windows

| Window | Time | Duration | Tasks |
|--------|------|----------|-------|
| Daily refresh | 06:00 AM IST | ~1 minute | Auto data refresh |
| Weekly check | Monday 09:00 AM | 15 minutes | Verify refresh, spot-check KPIs |
| Monthly maintenance | 1st Saturday | 2 hours | Update Desktop, review access, backup |
| Quarterly review | End of quarter | Half day | Stakeholder feedback, roadmap planning |
