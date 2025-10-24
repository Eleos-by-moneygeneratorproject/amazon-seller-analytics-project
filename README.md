# Data-Driven Strategy for Amazon Sellers: A Comprehensive Analytics Project

## Business Problem

Amazon sellers operate in a complex, competitive marketplace facing significant pressures. Intense price wars, high Amazon fees, and advertising costs squeeze profit margins. Managing inventory effectively is critical, with costly FBA storage fees for overstocking and lost sales plus ranking penalties for stockouts. Gaining visibility requires optimizing listings for the A9 search algorithm and winning the coveted Buy Box, heavily influenced by pricing, performance metrics, and customer reviews. Maintaining a positive reputation through review management is crucial but challenging. This project applies data analytics to address these interconnected challenges, aiming to shift from reactive problem-solving to proactive, data-informed strategies for sustainable profitability.

## Key Questions Addressed

* Who are the most valuable customer segments and how can they be targeted effectively?
* (Market Basket Analysis was inconclusive due to data sparsity, but the attempt is documented.)
* What is the overall sentiment of customer reviews and what are the key drivers of satisfaction/dissatisfaction?
* What is the forecasted demand (based on review volume proxy) for the upcoming period?

## Data Source

The primary dataset used is the "[Amazon Sales Dataset](https://www.kaggle.com/datasets/karkavelrajaj/amazon-sales-dataset)" from Kaggle, provided by user Karkavelrajaj. It contains over 1,400 records with product details, pricing, customer feedback (ratings and text reviews), and user information.

* **File:** `amazon.csv`

## Methodology & Tech Stack

This project employed a range of data analysis techniques to extract actionable insights:

* **Data Cleaning & Preprocessing:** Handled missing values, corrected data types (prices, ratings, discounts), and engineered features (`discount_amount`, `category_main`) using **pandas**.
* **Exploratory Data Analysis (EDA):** Investigated top products/categories, price/discount relationships, and customer feedback distributions using **pandas**, **matplotlib**, and **seaborn**.
* **Customer Segmentation:** Applied **K-Means clustering** (using **scikit-learn**) on user-level aggregated data (total spend, review count, average rating) after standardizing features with `StandardScaler` to identify distinct customer groups. The Elbow Method was used to determine the optimal number of clusters (k=4).
* **Market Basket Analysis:** Attempted Association Rule Mining using the **Apriori algorithm** (**mlxtend**). The analysis was inconclusive due to the sparsity of the data (users typically reviewed only one item per "basket").
* **Sentiment Analysis:** Utilized a lexicon-based approach (**TextBlob**) to calculate polarity and subjectivity scores for customer reviews (`review_content`). Aggregated sentiment by product to identify most-loved/hated items.
* **Time Series Forecasting:** Modeled weekly review counts (as a proxy for sales velocity) using an **ARIMA(1, 0, 1)** model (**statsmodels**) after confirming stationarity with the ADF test. Generated a 12-week forecast.

**Core Technologies:**
* Python 3
* Pandas
* NumPy
* Matplotlib & Seaborn
* Scikit-learn
* mlxtend
* TextBlob
* Statsmodels
* Google Colab (Development Environment)
* Google Sheets (Data Storage for BI)
* Looker Studio (Dashboarding)

## Key Findings & Visualizations

1.  **Four Distinct Customer Segments Identified:** K-Means clustering revealed four key groups:
    * **Champions (Cluster 3):** Small group (21 users), extremely high spend, high engagement, very satisfied.
    * **Engaged Loyalists (Cluster 2):** Small group (53 users), good spend, highest review frequency, satisfied.
    * **Potential Loyalists (Cluster 1):** Largest group (801 users), low spend/engagement, but satisfied. *Major opportunity for growth.*
    * **At-Risk Customers (Cluster 0):** Large group (316 users), low spend, low engagement, significantly lower satisfaction.

![Customer Segments](images/customer_segments.png)

2.  **Sentiment Analysis Highlights Product Strengths/Weaknesses:** While overall sentiment was generally positive, specific products showed very high or low polarity scores based on review text (after filtering for products with at least 2 reviews).

* **Example Most-Loved:** "boAt Micro USB 55 Tangle-free, Sturdy Micro US..."

![Top 5 most loved products](images/Top_5_most_loved_products_(Avg._Polarity_min_5_reviews).png)

* **Example Most-Hated:** "Samsung 80 cm (32 Inches) Wondertainment Serie..."

![Top 5 most hated products](images/Top_5_most_hated_products_(Avg._polarity_min_5_reviews).png)

3.  **Sales Volume Proxy is Stationary with Underlying Patterns:** The weekly review count (sales proxy) was found to be stationary. The ARIMA(1,0,1) model provided a stable forecast for the next 12 weeks.

![Weekly Review Count Forecast](images/Weekly%20Review%20Count%20Forecast%20(ARIMA).png)

## Actionable Recommendations

1.  **Implement Segment-Targeted Marketing:**
    * **Champions:** Launch a VIP program with exclusive perks & early access. Encourage testimonials.
    * **Engaged Loyalists:** Nurture with personalized content, new product alerts, and requests for reviews on new purchases.
    * **Potential Loyalists:** Use welcome email series, first/second purchase discounts, and product education to increase purchase frequency and engagement.
    * **At-Risk:** Conduct targeted surveys to understand dissatisfaction drivers (linking se  ntiment analysis). Offer proactive customer service and win-back promotions.
2.  **Prioritize Product Quality/Marketing based on Sentiment:**
    * Investigate products identified in the "Most-Hated" list for potential quality control issues or misleading descriptions.
    * Leverage positive sentiment from "Most-Loved" products in marketing copy and ad campaigns.
    * Analyze products with high sentiment but low sales ("Hidden Gems" quadrant from sentiment vs. sales plot) for potential marketing/visibility improvements.
3.  **Inform Inventory Planning with Forecast:** Use the ARIMA forecast's predicted weekly review volume (**approx. 7 per week**, based on your `forecast_df`) as a directional input for inventory planning over the next quarter, adjusting based on actual sales data and seasonality knowledge.

## How to Run This Project

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/Eleos-by-moneygeneratorproject/amazon-seller-analytics-project.git](https://github.com/Eleos-by-moneygeneratorproject/amazon-seller-analytics-project.git)
    cd amazon-seller-analytics-project
    ```
2.  **Ensure Data:** The `amazon.csv` file should be in the `/data/` directory.
3.  **Open the Notebook:** Upload the `.ipynb` file from the `/notebooks/` directory to Google Colab or open it using a local Jupyter Notebook environment.
4.  **Install Libraries (if needed):** The notebook includes installation commands (`!pip install ...`) for libraries like `gspread` and `mlxtend` if they are not already in your environment.
5.  **Run Cells Sequentially:** Execute the notebook cells from top to bottom. Note:
    * The code includes steps to authenticate with Google to write data to Google Sheets. You will need to follow the authentication prompts.
    * The Market Basket Analysis section will report that no significant rules were found.
    * The final output includes saving plots as PNG files.
