# E-COMMERCE FUNNEL ANALYSIS & CHECKOUT A/B TEST

# Project Overview

Northline is a US department store selling apparel, shoes, beauty, home and accessories online. During the 2025 holiday quarter, its website drew 380K+ visits, but only 3.5% ended in a purchase, and mobile shoppers converted far worse than desktop.

This project analyzes 3M+ website events from Q4 2025 (Oct 1 – Dec 31) using Python (pandas, SciPy, statsmodels) to find where shoppers abandon the purchase journey, measure what it costs, and evaluate whether a redesigned checkout (`checkout_v2`) should be launched, supporting a data-driven ship / no-ship decision.

# Key Analysis Areas

Insights and recommendations are provided across the following business dimensions:

* Data Quality & Cleaning - Detecting bot traffic, duplicate purchase events and internal test orders, and reconciling web tracking with the orders system

* Funnel Analysis - Step-by-step conversion from product view to purchase, by device, channel, customer type and shopping period

* Root Cause Analysis - Identifying why mobile shoppers abandon checkout, confirmed with natural experiments in the holiday calendar

* Opportunity Sizing - Estimating the revenue lost to the checkout drop-off under conservative, base and optimistic scenarios

* A/B Test Evaluation - Measuring the new checkout's impact on conversion, order value, revenue per user and payment errors

# Project Resources & Links

The Python notebook for Data Cleaning [here](https://github.com/ajoalenjeen/E-commerce-Funnel-Analysis-Checkout-A-B-Test/blob/0e74941fcf39395b0c9bce1c54c14a13ab4dc45e/Code/Data_Cleaning.ipynb)

The Python notebook for Funnel Analysis [here](https://github.com/ajoalenjeen/E-commerce-Funnel-Analysis-Checkout-A-B-Test/blob/1c5d0ba8d5e43d149fc3f183358bc4f29e24cde7/Code/Funnel_Analysis.ipynb)

The Python notebook for Opportunity Sizing [here](https://github.com/ajoalenjeen/E-commerce-Funnel-Analysis-Checkout-A-B-Test/blob/1c5d0ba8d5e43d149fc3f183358bc4f29e24cde7/Code/Sizing.ipynb)

The Python notebook for A/B Test Analysis [here](https://github.com/ajoalenjeen/E-commerce-Funnel-Analysis-Checkout-A-B-Test/blob/1c5d0ba8d5e43d149fc3f183358bc4f29e24cde7/Code/ab_testing.ipynb)


## Data Structure:

| Table | Rows | Description |
|---|---|---|
| events | 3.2M | GA4-style event log: page views, product views, add to cart, checkout steps, purchases |
| sessions | 384K | One row per visit: device, browser, channel, region |
| users | 240K | One row per shopper: region, loyalty membership |
| orders | 13.3K | One row per order: revenue, discount, shipping, returns |
| order_items | 25K | One row per item in each order |
| experiment_assignments | 12K | Which checkout version each test user saw |
| products | 720 | Product catalog |


# Overview of Findings:

Only 3.5% of Q4 visits ended in a purchase. Every checkout step performed the same on mobile and desktop except one: the shipping step, where mobile shoppers dropped out 11 points more often. The cause was a surprise $10.95 shipping fee on carts under $49, which cost an estimated $62K in the quarter. A redesigned checkout with a free-shipping progress bar raised conversion, especially on mobile, but smaller orders and an Apple Pay bug left revenue per user flat, so it is not ready to launch.

### Data Quality Insights:

* Removed 30 bot accounts generating ~202K fake events (6% of all events): desktop-only, active at 2–5 am, ~1 second per page and zero add-to-carts.

* Found 102 purchases logged twice (confirmation-page refreshes) that inflated revenue by $13.2K, and 38 internal test orders at $0.

* After cleaning, website tracking matched the orders system exactly: 13,301 orders and $1.68M gross revenue ($1.38M net of returns and cancellations).

### Funnel Insights:

<img src="https://github.com/ajoalenjeen/E-commerce-Funnel-Analysis-Checkout-A-B-Test/blob/39c9b510b25342c3d07b663587e6fb63da2b4cbb/Pictures/Screenshot%202026-11-19%20131828.png" 
     alt="Top 20 Cities" 
     width="1000">
* Out of every 100 visits, 63 view a product, 8 add to cart, 5 start checkout and 3.5 purchase. The biggest drop (view → cart, 12%) is normal browsing behavior.
  
<img src="https://github.com/ajoalenjeen/E-commerce-Funnel-Analysis-Checkout-A-B-Test/blob/554158b5cd19f610ee53d95fc6c666dedf4f4f35/Pictures/Screenshot%202026-11-19%20132027.png" 
     alt="Top 20 Cities" 
     width="1000">
     
* Mobile passes the shipping step at 77% vs. 89% on desktop; every other checkout step is within ~1.5 points across devices.

* Email converts best (5.2%) and social worst (1.7%); returning visitors convert 2x better than new visitors; Thanksgiving–Cyber Monday tripled traffic and converted at 4.8% vs. 2.8% at baseline.

* 1 in 5 buyers purchased on a later visit than when they first added to cart, so user-level conversion (5.2%) is higher than session-level (3.5%).

  <img src="https://github.com/ajoalenjeen/E-commerce-Funnel-Analysis-Checkout-A-B-Test/blob/554158b5cd19f610ee53d95fc6c666dedf4f4f35/Pictures/Screenshot%202026-11-19%20132123.png" 
     alt="Top 20 Cities" 
     width="1000">

* The drop is driven by the shipping fee: non-member mobile carts under $49 pass at 63% vs. 80% for $49+ carts, while loyalty members (always free shipping) show no drop.

* Natural experiments confirm it: on Free Shipping Day, mobile small carts jumped from 70% to 81%; during the express-only window ($19.95), even large carts dropped.


### Opportunity Sizing:

  <img src="https://github.com/ajoalenjeen/E-commerce-Funnel-Analysis-Checkout-A-B-Test/blob/791df21c2b763593170d6c4b3558b6d0d9d96fb2/Pictures/Screenshot%202026-11-19%20134432.png" 
     alt="Top 20 Cities" 
     width="1000">

* The mobile shipping-step drop-off cost an estimated $62K in Q4 (range $41K–$124K; $51K net of returns), about 3.7% of quarterly revenue.

* The estimate stays within $50K–$75K across different assumptions for order value and purchase rate.

### A/B Test Insights (checkout_v2, Nov 3 – Dec 21, 12,249 users):

  <img src="https://github.com/ajoalenjeen/E-commerce-Funnel-Analysis-Checkout-A-B-Test/blob/791df21c2b763593170d6c4b3558b6d0d9d96fb2/Pictures/Screenshot%202026-11-19%20134320.png" 
     alt="Top 20 Cities" 
     width="1000">

* The test was valid: a 50/50 split (SRM check p = 0.59) with balanced groups on device, browser and membership.

* Conversion rose from 70.3% to 74.1% (+3.8 pts, 95% CI +2.2 to +5.4, p < 0.001), led by mobile (+5.3 pts) and non-members (+4.1 pts).

* Average order value fell 5.3% ($128 → $122), from more small-cart buyers (harmless) and the removed "Complete the look" upsell (a real loss).

* Revenue per exposed user was flat: $93.58 vs. $94.26 (+0.7%, p = 0.74).

    <img src="https://github.com/ajoalenjeen/E-commerce-Funnel-Analysis-Checkout-A-B-Test/blob/791df21c2b763593170d6c4b3558b6d0d9d96fb2/Pictures/Screenshot%202026-11-19%20134338.png" 
     alt="Top 20 Cities" 
     width="1000">

* Apple Pay payment errors doubled (2.6% → 5.2%, p = 0.01) while other payment types were unchanged, pointing to an integration bug.


## Recommendation:

1️⃣ Hold checkout_v2: launching it now would roll out a payment bug for no measurable revenue gain.

2️⃣ Fix the Apple Pay integration in the new checkout before any rollout.

3️⃣ Restore the "Complete the look" recommendations to recover order value.

4️⃣ Re-test the fixed version in January for 3–4 weeks, judged on revenue per user (~3,500 users per group), then roll out mobile-first.

5️⃣ Remove the shipping-fee surprise by showing shipping cost on the cart page and testing a lower free-shipping threshold for mobile.

6️⃣ Grow loyalty membership (members never see the fee and convert 4.4% vs. 3.2%) and shift acquisition spend toward email and cart reminders.

## Tools & Methods:

* Python (pandas, NumPy, SciPy, statsmodels, matplotlib) 
* ordered funnel analysis 
* natural experiments 
* opportunity sizing 
* A/B testing (SRM check, two-proportion z-test, confidence intervals, Welch's t-test, Mann-Whitney U, bootstrap, Bonferroni correction, power analysis)

## 👤 Author

**Ajo Alen Jeen**,
MBA Candidate, Business Analytics — Pace University
[LinkedIn](https://www.linkedin.com/in/ajoalenjeen/) · [Portfolio](https://github.com/ajoalenjeen)
