
# Project: Marketing Channel Performance Analysis

## Objective
The Parts Avatar marketing team has allocated its budget across three primary channels for the first half of the year: Pay-Per-Click (PPC), Email Marketing, and Social Media advertising. The team needs a comprehensive analysis to understand the effectiveness and return on investment (ROI) of each channel.

Your goal is to ingest and integrate data from these disparate sources, calculate key performance indicators (KPIs), and deliver a clear report with actionable recommendations on where to focus future marketing spend.

## The Challenge
The data for this analysis is fragmented across four different files, each representing a piece of the customer journey. Your primary challenge is to create a single, unified view of marketing performance. You must decide how to join these datasets, handle any potential discrepancies, and calculate meaningful metrics that will drive business decisions. The conceptual part of this task is to define what "performance" means and to tell a clear story with the data.

## Datasets
* `data/ppc_spend.csv`: Daily spend on PPC campaigns.
* `data/email_campaigns.csv`: Data on email campaigns, including number of emails sent and clicks generated.
* `data/social_media_ads.csv`: Daily spend and performance data for social media ads.
* `data/website_conversions.csv`: A log of all sales conversions, including revenue and the marketing channel that sourced the customer.

## Your Tasks
1.  **Data Ingestion & Integration (ETL):**
    * Write a Python script (`src/process_data.py`) to load, clean, and merge the four data sources into a single, aggregated dataset. This dataset should be structured to allow for daily or weekly performance analysis per channel.

2.  **KPI Calculation:**
    * Using your integrated dataset, calculate the following key metrics for each marketing channel:
        * Total Spend
        * Total Clicks & Total Conversions
        * Total Revenue
        * Click-Through Rate (CTR, for social media)
        * Conversion Rate (Conversions / Clicks)
        * Cost Per Click (CPC)
        * Cost Per Acquisition (CPA, or cost per conversion)
        * Return on Investment (ROI) or Return on Ad Spend (ROAS)

3.  **Analysis & Insights:**
    * In a Jupyter Notebook or as part of your report, analyze the calculated KPIs to answer these questions:
        * Which channel has the highest ROI?
        * Which channel is the most and least expensive for acquiring a customer (CPA)?
        * Which channel is most effective at converting clicks into sales?
        * Is there any observable trend in performance over time for any of the channels?

4.  **Visualization & Recommendations:**
    * Create a summary dashboard or a set of clear visualizations (using Matplotlib, Seaborn, Plotly, etc.) that effectively communicate the performance of each channel.
    * Conclude your analysis in this README with a summary of your findings and provide a data-driven recommendation to the marketing team on how they should consider allocating their budget for the next quarter.

## Evaluation Criteria
* **Data Integration Logic:** Your approach to cleaning and merging the different data sources into a coherent model.
* **Accuracy of KPIs:** Correct calculation of standard marketing performance metrics.
* **Analytical Thinking:** Your ability to interpret the KPIs and extract meaningful, non-obvious insights from the data.
* **Communication & Visualization:** The clarity of your charts and the persuasiveness of your final recommendations.

## Disclaimer: Data and Evaluation Criteria
Please be advised that the datasets utilized in this project are synthetically generated and intended for illustrative purposes only. Furthermore, they have been significantly reduced in terms of sample size and the number of features to streamline the exercise. They do not represent or correspond to any actual business data. The primary objective of this evaluation is to assess the problem-solving methodology and the strategic approach employed, not necessarily the best possible tailored solution for the data. 

## Project Report
### Data Integration Logic
Upon observing the data, the `website_conversions.csv` file makes for an excellent base upon which I can build the aggregated dataset. This is the case because the other 3 files contain per-day metrics that are often overlapping (spend, clicks), whereas the `website_conversions.csv` file is on a per-conversion basis. Thus, to be able to cleanly merge all 4 files, I need to determine a method to aggregate `website_conversions.csv` such that it has one line for each combination of day and channel. By evaluating what metrics are needed to calculate the KPIs, I have determined that retaining individual `conversion_id`s and `revenue` entries is not necessary, as the KPIs are interested in total conversions and total revenue. In summary, the method I have decided to use to merge the 4 datasets is to take the `website_conversions.csv` file, group by `date` and `channel` then take the count of `conversion_id` and the sum of `revenue`. Next, I merge each of `email_campaigns.csv`, `ppc_spend.csv` and `social_media_ads.csv` onto the previous table using `date` and the corresponding `channel` value. Finally, I add a `week` helper column in the *yyyy-ww* format to make calculating weekly sums easier.

### KPI calculation
For certain KPIs, I elected to not calculate the KPI for all channels when it would result in dividing by zero because that is not an interesting result. For the full results and analysis, consult `notebooks/calculate_kpi.ipynb`.

### Visualization & Recommendations
![image](https://github.com/BenjaminRoderick/marketing-performance-analysis/blob/main/data/revenue_spend_over_time.png)

![image](https://github.com/BenjaminRoderick/marketing-performance-analysis/blob/main/data/conversions_clicks_over_time.png)

As we can observe in the above graphs, the PPC and Email channels produce the best results, with the Social Media lagging quite far behind. Indeed, Social Media may have been less expensive and generate more clicks than the other two channels, but the associated ROI was half that of PPC and the cost per acquisition was nearly 50% greater. Additionally, Social Media brought in barely a third of the revenue that Email and PPC were able to bring in individually.

As for PPC, despite being the channel with the most total spend by far, the KPIs indicate that it is more than worth it. Of all channels, it brings in the most revenue and the most conversions, with an excellent ROI of 393%.

Email campaigns are outclassed by PPC in both the revenue and acquisition metrics and outclassed by Social Media in terms of clicks, but they have one massive advantage over the other two channels: the associated costs are zero. Consequently, they have an infinite ROI and a zero-dollar cost per acquisition. This means that any marketing stragtegy that is focused on maximizing revenue and acquisition must invest heavily in email campaigns.

Finally, my recommendation regarding budget allocation for the next quarter would be to continue or increase the emphasis on the Email channel and increase budget for PPC campaigns while reducing Social Media spending. This strategy maintains the broad reach of multi-platform marketing by not over-investing in one channel whilst at the same time ensuring that the majority of the budget and manpower are focused on the programs with the best return on investment and gross revenue.
