# Hospitality Analytics Dashboard — Power BI

A comprehensive Power BI analytics dashboard for a luxury hotel chain operating 7 Taj properties across India. Provides multi-year performance tracking (2021–2025), financial analysis, guest satisfaction monitoring, booking channel insights, and membership analytics.

## Overview

| Detail | Value |
|--------|-------|
| Hotels | 7 Taj Properties across 7 Cities |
| Records | 100,000 Bookings |
| Date Range | January 2021 – September 2025 |
| Customers | 200 Unique Customers, 10 Countries |
| Security | Row-Level Security by Hotel Manager |

## Key Features

- Revenue tracking across properties and time periods (Year → Quarter → Month drill-down)
- Hotel performance comparison by city, category, and manager
- Guest satisfaction monitoring through feedback metrics
- Booking channel and device preference analysis
- Member vs. Non-Member revenue analysis
- Row-Level Security — managers see only their property data, executives see all
- Star Schema data model with 20+ DAX measures

## Dashboard Pages

1. **Home Page** — Navigation hub
2. **Executive Summary** — Portfolio-level KPIs
3. **Revenue Trends** — Time-based revenue analysis
4. **Guest Satisfaction** — Feedback and ratings
5. **Channel Analysis** — Booking source breakdown
6. **Member Analysis** — Loyalty program insights

## Data Model

Star Schema design with:
- 1 Fact Table: `fct_Bookings` (100K records)
- 6 Dimension Tables: Date, Hotel, Manager, Finance, Feedback, Online Booking

## Tech Stack

- **Visualization:** Power BI Desktop (1920×1080 canvas)
- **Data Transformation:** Power Query (M)
- **Calculations:** DAX (20+ measures)
- **Security:** Row-Level Security (RLS)
- **Deployment:** Power BI Service with scheduled refresh

## Repository Structure

```
├── Business_Requirements_Document.md   # BRD with objectives and scope
├── Functional_Specification.md         # Solution design and data model
├── Star_Schema_Design.md               # Data model details
├── DAX_Measures.md                     # All DAX calculations
├── Power_Query_Transformations.md      # ETL logic
├── RLS_Implementation.md               # Row-Level Security setup
├── PowerBI_Build_Guide.md              # Step-by-step build instructions
├── *_Page_Design.md                    # Individual page designs
├── Administrator_Guide.md              # Admin documentation
├── User_Manual.md                      # End-user guide
├── Deployment_Guide.md                 # Publishing and refresh setup
├── Testing_Checklist.md                # QA validation steps
├── Support_Runbook.md                  # Troubleshooting guide
└── mockups/                            # HTML and PNG page mockups
```

## Getting Started

1. Review the [Business Requirements Document](Business_Requirements_Document.md)
2. Follow the [Power BI Build Guide](PowerBI_Build_Guide.md) for step-by-step implementation
3. Refer to [DAX Measures](DAX_Measures.md) for all calculations
4. Use the [Deployment Guide](Deployment_Guide.md) for publishing

## Methodology

Built using AI-DLC (AI-Driven Development Lifecycle) methodology.

---

*KSR Datavizon Hospitality Analytics Challenge — June 2026*
