# Amazon Laptop Data Scraper

## Project Overview

This project is developed to scrape laptop product information from Amazon India and store the extracted data in a timestamped CSV file.

The scraper collects the following information for each laptop product:

* Product Title
* Product Price
* Product Rating
* Product Image URL
* Product Type (Ad / Organic)

The data is collected from multiple pages of Amazon search results and saved in a structured format for further analysis.

---

## Problem Statement

Write a Python script to scrape laptop product data from Amazon India and store the extracted information in a CSV file.

### Required Fields

* Image URL
* Product Title
* Product Rating
* Product Price
* Ad / Organic Result

### Output Requirement

* Save the output in CSV format.
* The output file should contain a timestamp in its filename.

---

## Environment Setup

### Step 1: Create a Virtual Environment

```bash
python -m venv env
```

### Step 2: Activate the Virtual Environment

PowerShell:

```powershell
& .\env\Scripts\Activate.ps1
```

### Step 3: Install Required Packages

```bash
pip install requests beautifulsoup4 pandas
```

### Step 4: Install Jupyter and IPython Kernel

```bash
pip install jupyter ipykernel
```

### Step 5: Register the Virtual Environment as a Jupyter Kernel

```bash
python -m ipykernel install --user --name=env --display-name "Python (env)"
```

### Step 6: Reload VS Code

Open Command Palette:

```text
Developer: Reload Window
```

Select:

```text
Python (env)
```

as the notebook kernel.

---

## Libraries Used

```python
from bs4 import BeautifulSoup
import pandas as pd
import requests
import time
from datetime import datetime
```

### Library Purpose

| Library       | Purpose                         |
| ------------- | ------------------------------- |
| requests      | Send HTTP requests to Amazon    |
| BeautifulSoup | Parse HTML content              |
| pandas        | Create DataFrame and export CSV |
| datetime      | Generate timestamped file names |
| time          | Add delay between requests      |

---

## Webpage Fetching Strategy

The script uses custom HTTP headers to mimic a real browser request and reduce the chances of being blocked by Amazon.

```python
headers = {
    "User-Agent": (
        "Mozilla/5.0 (Windows NT 10.0; Win64; x64) "
        "AppleWebKit/537.36 (KHTML, like Gecko) "
        "Chrome/137.0.0.0 Safari/537.36"
    ),
    "Accept-Language": "en-IN,en;q=0.9"
}
```

### Header Explanation

#### User-Agent

Used to make the request appear as if it is coming from a real web browser rather than a bot.

#### Accept-Language

```text
en-IN,en;q=0.9
```

This tells Amazon that:

* Preferred language: English (India)
* Secondary preference: English

---

## Scraping Workflow

### 1. Fetch Amazon Search Pages

The script sends requests to multiple pages of Amazon laptop search results.

Example:

```text
https://www.amazon.in/s?k=laptop&page=1
https://www.amazon.in/s?k=laptop&page=2
...
```

### 2. Parse HTML Content

BeautifulSoup is used to parse the HTML response.

```python
soup = BeautifulSoup(response.text, "html.parser")
```

### 3. Extract Product Containers

```python
products = soup.select(
    'div[data-component-type="s-search-result"]'
)
```

This selector identifies each laptop product card on the page.

### 4. Extract Product Information

The following information is collected for each laptop:

* Title
* Price
* Rating
* Image URL
* Product Type (Ad / Organic)

### 5. Store Data in Lists

Extracted information is stored in Python lists and later combined into a Pandas DataFrame.

### 6. Create DataFrame

```python
df = pd.DataFrame({
    "Title": titles,
    "Price": prices,
    "Rating": ratings,
    "Image": images,
    "Product_Type(Ad/Organic)": product_types
})
```

### 7. Save Data with Timestamp

```python
timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")

file_name = f"amazon_laptop_data_{timestamp}.csv"

df.to_csv(file_name, index=False)
```

Example Output:

```text
amazon_laptop_data_20260603_194530.csv
```

---

## Output

The scraper generates a timestamped CSV file containing laptop product information.

Example:

```text
amazon_laptop_data_20260603_194530.csv
```

---

## Project Structure

```text
Amazon-Scraper/
│
├── amazon_scraper.ipynb
├── README.md
├── requirements.txt
├── amazon_laptop_data_YYYYMMDD_HHMMSS.csv
└── env/
```

---

## Conclusion

This project successfully automates the extraction of laptop product information from Amazon India using Python, Requests, BeautifulSoup, and Pandas. The collected data is stored in a timestamped CSV file, making it suitable for data analysis, reporting, and further processing.
