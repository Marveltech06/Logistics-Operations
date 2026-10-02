# Logistics-Operations
An end-to-end analysis of trucking operations (Jan 2022 – Dec 2024): revenue, fleet, drivers, safety, lanes, fuel and service performance.
![](Logisticimage.avif)


## Table of Contents
- Project Overview
- Business Problem and Objectives
- Data Description
- Tools and Methodology
- Data Cleaning and Preparation
- Metric Definitions
- Exploratory Analysis and Key Insights
- Dashboards
- Recommendations and Informed Decision-Making

## 1. Project Overview
A trucking company generates data across loads, trips, drivers, trucks, fuel, maintenance, safety and delivery events, but the tables sit in separate files. This project joins 14 relational tables and builds four decision-focused dashboards that show where the business earns money, where it loses it, and what to do about it.
Scope: 3 years (Jan 2022 – Dec 2024), 85,410 completed loads, 150 drivers, 120 trucks.

## 2. Business Problem and Objectives
Problem statement: Management lacks one integrated view of commercial, fleet, driver and service performance, so problems such as late deliveries and weak lanes are not visible or prioritized.
**Primary objective**: Turn raw operational data into insights that support cost, service and safety decisions.

*Specific objectives*
- Measure overall financial performance: revenue, revenue per mile, fuel cost, customer mix.
- Assess fleet health: utilization, downtime, maintenance cost by truck type and model year.
- Evaluate driver performance and safety: on-time delivery, fuel efficiency, preventable incidents, claims.
- Identify the most and least profitable lanes, and the fuel and service issues (detention, on-time rate) that erode margin.
- Provide prioritized, evidence-based recommendations.

*Key business questions*
- Is revenue growing, and who generates it?
- Why are deliveries late?
- Which lanes and trucks cost the most per mile?
- Where do preventable safety costs come from?
- What drove the change in fuel cost?
- Interactive Logistics Operations Dashboard | Truck Utilization, Revenue, Maintenance & Operational Performance

Here is the snapshot of the dashboard
![](OlatunbosunS.FCapstoneProject.png)
Click here to interact with the dashboard: [here](https://olatunbosunsamuellogisticdashboard.netlify.app/)

## 3. Data Description

The data comes from 14 CSV files in a relational (star-like) structure.

| Group | Tables | Role |
|---|---|---|
| Dimensions | `customers`, `routes`, `drivers`, `trucks`, `trailers`, `facilities` | Descriptive attributes |
| Transactions | `loads`, `trips`, `fuel_purchases`, `delivery_events`, `maintenance_records`, `safety_incidents` | Events and costs |
| Aggregates | `truck_utilization_metrics`, `driver_monthly_metrics` | Pre-summarized monthly metrics |

### Key relationships

- `loads` → `customers` (`customer_id`), `routes` (`route_id`)
- `trips` → `loads` (`load_id`), `drivers` (`driver_id`), `trucks` (`truck_id`), `trailers` (`trailer_id`)
- `fuel_purchases`, `delivery_events`, `safety_incidents` → `trips` (`trip_id`)
- `delivery_events` → `facilities` (`facility_id`)
- `maintenance_records`, `truck_utilization_metrics` → `trucks` (`truck_id`)
- `driver_monthly_metrics` → `drivers` (`driver_id`)

## 4. Tools and Methodology
- Power BI: Data profiling, cleaning, joining, and aggregation.
- HTML: Interactive dashboards with filters and slicers.
- Power BI: Data modelling, DAX measures, and dashboard development.

Method: Profile → Clean → Validate → Join → Aggregate → Visualize → Interpret → Recommend.

## 5. Data Cleaning and Preparation
- Duplicates: No duplicate primary keys were found.
- Missing values: Retained where appropriate; excluded only when they affected specific analysis.
- Dates: Converted to datetime and used to create monthly/yearly trends.
- Revenue: Calculated as revenue + fuel_surcharge + accessorial_charges.
- Load scope: Only Completed loads were included in revenue and load-count analysis.
- Experience: Drivers grouped into 0–2, 3–5, 6–10, and 10+ years for safety analysis.
- Lane analysis: Origin and destination cities were combined to calculate lane-level revenue per mile.
- Data quality: Some metrics showed highly similar values across groups, suggesting the dataset may contain synthetic or standardized data. Non-discriminating visuals were therefore excluded.

## 6. Metric Definitions

| Metric | Definition |
|---|---|
| **Total Revenue** | Sum of revenue, fuel surcharge, and accessorial charges for completed loads |
| **Revenue per Mile** | Total revenue ÷ total actual distance miles |
| **Fleet MPG** | Total miles ÷ total fuel gallons used (weighted fleet MPG, not an average of truck-level MPG) |
| **Delivery / Pickup On-Time %** | Mean of `on_time_flag` for events of the respective type |
| **Fuel % of Revenue** | Total fuel cost ÷ total revenue |
| **Utilization %** | Mean `utilization_rate` for active trucks only |
| **Maintenance Cost per Mile** | Total maintenance cost ÷ total miles |
| **Preventable / At-Fault %** | Number of flagged incidents ÷ total incidents |
| **Drivers Terminated %** | Non-active drivers ÷ total drivers; represents the share of the current driver population that is non-active, not an annual turnover rate |
| **Driver Score** | 40% On-Time + 30% MPG + 30% Safety score, using min-max scaling (0–100) for each component; active drivers only |

## 7. Exploratory Analysis and Key Insights

### 7.1 Executive Overview

| KPI | Value |
|---|---:|
| **Revenue (3 years)** | **$298.6M** (~$8.3M per month) |
| **Completed Loads** | **85,410** |
| **Revenue per Mile** | **$2.44** |
| **Fleet MPG** | **6.45** |
| **Delivery On-Time Rate** | **44.6%** |

### Note

- The fleet generated approximately **$298.6M in revenue** across 85,410 completed loads over the three-year period.
- Average revenue was approximately **$8.3M per month**, indicating substantial operating volume.
- The fleet generated **$2.44 in revenue per mile**, providing a useful measure of revenue efficiency.
- Fleet fuel efficiency was **6.45 MPG**, calculated using total miles divided by total fuel consumed.
- The **44.6% delivery on-time rate** indicates that fewer than half of completed delivery events met the defined on-time threshold, making delivery reliability an important area for further investigation.

## Insights

- Revenue is flat. Monthly revenue sits near $8.3M with no growth trend, so profit gains must come from cost and pricing, not volume.
- Contract customers bring about 38% of revenue. The top customer is about $10.4M (roughly 3.5%), so customer concentration risk is low.
- Fuel's share of revenue fell from about 34% to about 30% over the period, which is the main source of margin improvement.

### 7.2 Fleet and maintenance
- Only 92 of 120 trucks (77%) are active: 15 are in maintenance and 13 are inactive. About a quarter of the fleet is not earning.
- Utilization of active trucks is about 83%. Maintenance cost totals $5.73M (about $0.048 per mile) with about 72,000 downtime hours.
- Maintenance cost is spread evenly across types, so no single repair category dominates.
- Model-year 2021 trucks cost about $0.056 per mile, roughly 2× the 2019 trucks. Age alone does not explain cost, so these units warrant a closer look for warranty, spec or quality issues.

### 7.3 Drivers and safety
- 170 incidents, $2.65M in claims. 37.6% were preventable and 31.8% at-fault.
- Preventable incidents are the largest controllable cost and are spread across incident types, so a general coaching program fits better than a single-issue fix.
- Experience shows no clear protective effect. Claim cost per driver is about $13k for the 3–5 and 6–10 year bands and $19–21k for the 0–2 and 10+ bands (the 0–2 band has only 7 drivers).
- 26 of 150 drivers (17.3%) are terminated.
- Late deliveries are systemic, not a few bad drivers. Even the highest-scoring driver is only about 50% on-time.

### 7.4 Lanes, fuel and service
- Revenue per mile ranges from $3.62 (Philadelphia–New York) to $1.70 (Charlotte–Denver), a 2× spread. Several of the weakest lanes are long hauls, but not all.
- Fuel price fell in two steps (about $4.20 → $3.85 → $3.65, a drop of about 13%) while MPG stayed near 6.45. Savings came from price, not efficiency.
- Delivery on-time is 44.6% against 66.7% for pickups. More than half of deliveries are late.
- Detention averages about 91 minutes per stop at every facility (about 260,000 hours in total). Because it is uniform, the cause is likely appointment and dock scheduling, not one problem site.

## Summary of the five most important findings
1. More than half of deliveries are late (44.6% on-time).
2. Revenue is flat; margin gains came from lower fuel prices, not efficiency.
3. Lane profitability varies 2×, so pricing is uneven.
4. A quarter of the fleet is inactive or in maintenance; 2021 trucks are the costliest to maintain.
5. Over a third of safety incidents were preventable.

## 8. Dashboards

| Dashboard | Focus | Slicers |
|---|---|---|
| **1. Executive Overview** | Revenue, customers, and fuel cost share | Year, Customer Type |
| **2. Fleet & Maintenance** | Fleet utilization, downtime, and maintenance cost per mile | Year, Home Terminal |
| **3. Drivers & Safety** | Incidents, claims, and driver scorecard | Year, Experience Band |
| **4. Lanes, Fuel & Service** | Lane profitability, fuel pricing, and on-time performance | Year, Booking Type |

Here is the snapshot of the dashboard
![](OlatunbosunS.FCapstoneProject.png)
Click here to interact with the dashboard: [here](https://olatunbosunsamuellogisticdashboard.netlify.app/)

## 9. Recommendations and Informed Decision-Making

| # | Decision | Evidence | Recommended Action | KPI to Track |
|---|---|---|---|---|
| 1 | **Improve delivery on-time performance** | 44.6% on-time; approximately 91 minutes of detention across sites | Introduce appointment scheduling and dock-window agreements with customers; review how scheduled times are set; add detention billing where contracts allow | Delivery on-time %, detention minutes per stop |
| 2 | **Reprice or exit weak lanes** | Revenue per mile ranges from $1.70 to $3.62 | Review lower-performing lanes against their `base_rate_per_mile`; renegotiate rates, add fuel surcharges, or reduce volume where appropriate | Revenue per mile by lane |
| 3 | **Increase utilization of inactive trucks** | 28 of 120 trucks are not active | Assess each inactive truck for repair, redeployment, or sale; establish a target for active fleet share | Active trucks %, utilization |
| 4 | **Investigate 2021 model-year maintenance costs** | Approximately 2× the cost per mile compared with 2019 models | Review warranty claims and repair history; evaluate replacement or warranty-related actions where appropriate | Maintenance cost per mile by model year |
| 5 | **Reduce preventable incidents** | 37.6% preventable incidents; $2.65M in claims | Implement targeted driver coaching, telematics alerts, and periodic reviews by incident type | Preventable incidents per 100k miles, claims paid |
| 6 | **Protect fuel-cost savings** | Fuel prices fell approximately 13% while MPG remained flat | Use fuel cards and preferred fuel stops; set MPG improvement targets through idle reduction and route optimization | Fuel % of revenue, MPG |
| 7 | **Pursue targeted revenue growth** | Monthly revenue remained relatively flat with low customer concentration | Prioritize stronger lanes and customer segments using capacity released through improved fleet utilization | Monthly revenue, load count |

## 👨‍💻 Author
### Folagbade Olatunbosun Samuel
- 💼 LinkedIn:https://www.linkedin.com/in/olatunbosun-folagbade-559151243/
- 📧 Email:Folagbadeolatunbosun@gmail.com

