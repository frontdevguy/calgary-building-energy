# Energy Consumption and Efficiency Analysis

## 📌 Project Overview
This project analyzes **energy consumption, emissions, and efficiency** across various property types, focusing on **fire stations and office buildings**

---

## 🛠️ Methodology
### 1️⃣ Data Cleaning and Preprocessing
- Standardized column names and removed inconsistencies in property type names.
- **Handled missing values** by imputing medians for numerical fields and mode values for categorical data.
- **Applied regex-based transformations** to clean numerical columns, postal codes, and address fields.

### 2️⃣ Data Cleaning Using Regex
#### 🔹 Extracting Float Values from Numeric Columns
Many **numeric columns** contained **comma-separated numbers and inconsistent formatting**. To standardize these values, **Regex** was used to extract the current value. Checks if there is a fraction part and returns the result float accordingly

```python
import re

def extract_float_from_text(value):
    pattern = re.compile(r'(?P<Int>[\d,]+)(?:\.(?P<Frac>\d+))?')
    match = pattern.match(str(value))
    
    if match:
        int_part = match.group("Int").replace(",", "")
        frac_part = match.group("Frac")
        
        if frac_part:
            return float(f"{int_part}.{frac_part}")
        else:
            return float(int_part)
            
    return value
```


#### 🔹 Clean the POSTAL CODE
The Postal code column contained several inconsistencies. To put this in the standard format. We removed all spaces and extracted the first 6 alphanumeric characters. We return the 6 characters in the standard format

```python
import re

def extract_postal_code_from_text(value):
    match = re.search(r"[A-Za-z0-9]{6}", str(value).strip().replace(" ", ""))
    
    if match:
        postal_code = match.group(0)
        return f"{postal_code[:3].upper()} {postal_code[3:].upper()}"

    return value
```

#### 🔹 Clean the Address One
Some address one columns contain the city and province. We used Regx to remove these values since we have a dedicated columns for them

```python
import re

def clean_address_one(value):
    return re.sub(r"\b(Aberta|Calgary)\b", "", value, flags=re.IGNORECASE).strip()
```

## Exploratory Data Analysis (EDA)
- **Computed summary statistics** for numerical columns, highlighting outliers and extreme values.
- Used **correlation analysis** to determine relationships between **energy use, emissions, and building size**.
- Conducted a **T-test** to compare **Site Energy Use Intensity (EUI)** between **fire stations and office buildings**.
- **Heatmaps and bar charts** were used to visualize **energy usage across different property types**.


## Identifying Outliers and Anomalies
- Applied **Interquartile Range (IQR) and Standard Deviation (STD) methods** to detect and replace outliers.
- Compared **yearly trends** to identify fluctuations in energy usage.


## Challenges Faced
### Lack of Monthly Energy Data
- The dataset contained only **yearly energy data**, making it **impossible to analyze seasonal variations in energy consumption**.
- Without **monthly or seasonal breakdowns**, we could not determine how **heating and cooling demands fluctuate throughout the year**.

### High Null Values in ENERGY STAR Score
- The **ENERGY STAR Score field contained a significant number of missing values**, making it **unusable for benchmarking energy efficiency**.
- This required us to **exclude it from correlation analysis and energy performance comparisons**, limiting deeper insights into building efficiency.


## Insights Gained
- **Fire stations consume a significant amount of energy (1.21 GJ/m²)** due to **24/7 operations**, but their **EUI is lower than recreational facilities**.
- **Office buildings (1.30 GJ/m²) are more energy-intensive per square meter**, but they have **greater flexibility for energy optimization**.
- **Energy use and emissions have a strong correlation (0.76)**, reinforcing that **reducing energy consumption directly lowers emissions**.
- **Larger buildings consume more energy (0.73 correlation), but size alone does not determine efficiency**—smaller buildings can still be highly inefficient.
- **Seasonal energy variations were difficult to analyze due to yearly data**, but fire stations showed **stable energy use**, while **offices likely fluctuate with temperature demands**.