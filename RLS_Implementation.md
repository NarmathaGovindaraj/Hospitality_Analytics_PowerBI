# Row-Level Security (RLS) Implementation
## Complete Implementation Guide

---

## 1. RLS Architecture

### Design Pattern: Dynamic Single-Role

Instead of creating 7 individual roles (one per manager), we use a single dynamic role
that filters based on the logged-in user's email address via `USERPRINCIPALNAME()`.

```
User Login → USERPRINCIPALNAME() returns email
    ↓
dim_Manager[ManagerEmail] filtered to matching row
    ↓ (Bidirectional R3)
dim_Hotel filtered to matching hotel
    ↓ (R2 propagation)
fct_Bookings filtered to matching hotel bookings
    ↓ (R4, R5, R6 propagation)
dim_Finance, dim_Feedback, dim_OnlineBooking all filtered
```

---

## 2. Prerequisites

Before implementing RLS:

1. **Relationship R3 must be Bidirectional:**
   - dim_Hotel[Hotel_Name] ↔ dim_Manager[HotelName]
   - Cardinality: 1:1
   - Cross-filter direction: Both

2. **dim_Manager[ManagerEmail] must be lowercase:**
   - Applied in Power Query: `Text.Lower(Text.Trim(email))`
   - USERPRINCIPALNAME() returns lowercase in Power BI Service

3. **Manager emails must match Azure AD UPN:**
   - aman.mehta@tajhotels.com must be the actual UPN in Azure AD
   - If emails differ, create a mapping table

---

## 3. Step-by-Step Implementation in Power BI Desktop

### Step 3.1: Verify Relationship R3

1. Go to **Model View**
2. Click the line between dim_Hotel and dim_Manager
3. Verify:
   - From: dim_Hotel[Hotel_Name]
   - To: dim_Manager[HotelName]
   - Cardinality: One to one (1:1)
   - Cross filter direction: **Both**
   - Make this relationship active: **Yes**

### Step 3.2: Create the RLS Role

1. Go to **Modeling tab** → **Manage Roles**
2. Click **Create** (New role)
3. Role Name: `HotelManager`
4. In the table list, select **dim_Manager**
5. In the DAX filter expression box, enter:

```dax
[ManagerEmail] = USERPRINCIPALNAME()
```

6. Click the **checkmark** ✓ to validate
7. Click **Save**

### Step 3.3: Test RLS in Desktop

1. Go to **Modeling tab** → **View as**
2. Check the checkbox for **"HotelManager"** role
3. In "Other user" field, enter: `aman.mehta@tajhotels.com`
4. Click **OK**
5. **Verify:**
   - Dashboard shows only Taj Mahal Palace data
   - Revenue, bookings, feedback — all Mumbai only
   - Page 3 (Manager View) shows "Aman Mehta — Taj Mahal Palace"
6. Click **Stop viewing** when done
7. Repeat test for each manager email

### Step 3.4: Test All Managers

| Test | Enter Email | Expected Hotel | Expected City |
|------|-------------|----------------|---------------|
| 1 | aman.mehta@tajhotels.com | Taj Mahal Palace | Mumbai |
| 2 | kavya.shah@tajhotels.com | Taj Lake Palace | Udaipur |
| 3 | rohan.das@tajhotels.com | Taj Falaknuma Palace | Hyderabad |
| 4 | sneha.roy@tajhotels.com | Taj Bengal | Kolkata |
| 5 | arjun.rao@tajhotels.com | Taj West End | Bengaluru |
| 6 | priya.nair@tajhotels.com | Taj Exotica | Goa |
| 7 | sanjay.iyer@tajhotels.com | Taj Coromandel | Chennai |

---

## 4. Deployment to Power BI Service

### Step 4.1: Publish the Report

1. Home → **Publish** → Select workspace
2. Wait for successful publication

### Step 4.2: Assign Members to Role

1. In Power BI Service, go to the **workspace**
2. Find the **dataset** (not the report)
3. Click **⋯** → **Security**
4. Select the **HotelManager** role
5. Add members one by one:
   - aman.mehta@tajhotels.com
   - kavya.shah@tajhotels.com
   - rohan.das@tajhotels.com
   - sneha.roy@tajhotels.com
   - arjun.rao@tajhotels.com
   - priya.nair@tajhotels.com
   - sanjay.iyer@tajhotels.com
6. Click **Add** for each

### Step 4.3: Executive Access

- **Do NOT assign executives to any role**
- Users without a role assignment see ALL data by default
- This gives full portfolio visibility to leadership

### Step 4.4: Test in Service

1. Click **⋯** on dataset → **Security**
2. Click **"Test as role"** next to HotelManager
3. Enter each manager's email and verify data filtering
4. Also test with "None" (no role) to confirm executives see all data

---

## 5. Security Validation Checklist

| # | Test Case | Expected Result | Pass/Fail |
|---|-----------|-----------------|-----------|
| 1 | Manager A logs in | Sees only Hotel A data | |
| 2 | Manager A cannot see Hotel B data | Zero rows from other hotels | |
| 3 | Revenue total matches hotel-specific total | Sum equals hotel's share only | |
| 4 | Feedback shows only hotel-specific reviews | No other hotel reviews visible | |
| 5 | Finance data filtered correctly | Cost/Revenue for hotel only | |
| 6 | Executive (no role) sees all 7 hotels | Full 100K records visible | |
| 7 | Unknown email sees no data | Blank dashboard (no access) | |
| 8 | Manager cannot see other managers' names | Only their own row in dim_Manager | |
| 9 | Drill-through respects RLS | Drilled data stays within hotel | |
| 10 | Export to Excel respects RLS | Exported data is filtered | |

---

## 6. Troubleshooting

| Issue | Cause | Solution |
|-------|-------|----------|
| Manager sees all data | R3 not bidirectional | Edit R3, set Cross-filter = Both |
| Manager sees no data | Email case mismatch | Ensure lowercase in both dim_Manager and Azure AD |
| USERPRINCIPALNAME() returns blank | Testing in Desktop | Use "View as" with "Other user" field |
| RLS works in Desktop but not Service | Role not assigned | Assign user to role in Dataset Security |
| Executive sees no data | Executive assigned to role | Remove executive from HotelManager role |
