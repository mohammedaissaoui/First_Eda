# Apple Products Price & Market EDA

My first end-to-end Exploratory Data Analysis (EDA) project — cleaning a messy real-world-style dataset of Apple product listings from Amazon and Flipkart, then exploring it to answer real questions about pricing, discounts, and reviews.

## About the Dataset

- **82,000 rows, 14 columns** — Apple product listings (iPhone, Mac, iPad, Watch) scraped from Amazon and Flipkart.
- Columns include price (USD & MAD), discount percentage, rating, review count, stock status, condition, and posting date.
- **Note:** the dataset shows some signs of being partially generated for practice (e.g. an unusually exact 50/50 platform split). Findings below are still valid patterns _within this dataset_, but should be read as a learning exercise rather than a real market report.

## Cleaning Summary

The raw data had several real quality issues that needed fixing before analysis:

- **Dates** were stored in 3 different formats mixed in the same column — standardized with `pd.to_datetime`.
- **Prices** (`Current_Price_USD`) were stored as text because ~1% of rows contained placeholder values like `"--"` — converted to numbers, and those rows were dropped rather than filled (guessing a price would have created false findings).
- **Platform** and **Condition** columns had inconsistent capitalization and stray spaces (e.g. `"amazon"`, `" Amazon"`, `"AMAZON "`) — cleaned and standardized.
- **Sale_Event** was missing in 92% of rows. Dropping them would have removed most of the dataset, so missing values were filled with `"Unknown"` instead, treating it as its own valid category.
- **2,000 duplicate rows** were removed.

## Key Findings

**1. Prices are right-skewed — a handful of expensive products pull the average up.** Most listings fall between $433–$990, but the mean ($783) sits well above the median ($700), and ~6% of listings are statistical outliers on the high end — most likely higher-end Mac models.
![[Price distribution.png]]
<img width="1024" height="469" alt="Price distribution" src="https://github.com/user-attachments/assets/56ad4d44-d457-4029-8b19-71e7c188a248" />


**2. Discount levels form three clear tiers by product category.** iPad and Watch have the highest discounts (~29% and ~25%, statistically no real difference between them). iPhone sits in the middle (~18%). Mac has the lowest discounts by far (~11%), and is the least likely to go on sale.
![[Discount by category.png]]

**3. Platform (Amazon vs Flipkart) barely affects price.** For every product category, the price gap between platforms was tiny — under $9 in every case, and statistically negligible. This dataset shows no evidence that one platform prices Apple products differently than the other.
![[Price by category and platform.png]]

![[Price by category and platform Boxplot.png]]

**4. Mac is the only category whose price rose over time.** Between 2020 and 2026, average Mac prices climbed from ~$919 to ~$1,364, while iPhone and Watch prices declined and iPad stayed roughly flat. This should be read with some caution — 2020 has very few Mac listings (n=57), and part of the rise could be newer, pricier Mac models entering the catalog over time rather than existing models getting more expensive.
![[Price trend by yea.png]]


**5. Bigger discounts loosely line up with more reviews — but discount likely isn't the cause.** Items with many reviews almost never have a small discount, which produces a moderate positive correlation (r = 0.62). The more likely explanation: products that have been listed longer simply have more time to both collect reviews _and_ get marked down — not that a discount itself drives people to leave reviews. This dataset doesn't include a "time listed" column, so this stays a plausible explanation rather than a proven one.
![[Discount vs review count.png]]

6. **Rating has no real relationship with price.** Cheap and expensive products get similar ratings across the board (r = 0.11, essentially no relationship).
![[Price vs Rating.png]]

## Limitations

- The dataset likely contains some artificially generated patterns, so findings reflect this specific file, not the real Apple resale market.
- 2020 has a very small number of listings compared to later years, which weakens any year-by-year comparison involving that year.
## Files

- `apple_eda.ipynb` — full cleaning and analysis notebook, with step-by-step reasoning.
- `data/raw.csv` — original, untouched dataset.
- `data/cleaned.csv` — cleaned dataset used for analysis.
- `images/` — saved charts referenced above.

## Tools Used

Python, Pandas, NumPy, Matplotlib, Seaborn — cleaning, statistics, and visualization done entirely in Jupyter Notebook.
