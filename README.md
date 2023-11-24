# Amsterdam AirBnB Recommendations System

<img src="https://i.imgur.com/vr42hjE.png" height="50%" alt="Amsterdam's De Gooyer Windmill"/>

<h2>🗺️Project Description</h2>
The AirBnB Amsterdam Recommendation System is a data science portfolio project that aims to provide tourists with personalized recommendations for the best AirBnB listings based on proximity to their planned tourists stops in Amsterdam. The project involves data cleaning, currency conversion and aggregate calculations in Python/NumPy. The resulting recommendation system is then turned into an interactive app via Streamlit.

<h2>💻Languages/Libraries and Utilities Used</h2>

- <b>Python (NumPy)</b> 
- <b>Streamlit</b>

<h2>📝Dataset Description:</h2>
Amsterdam subset of data from: http://insideairbnb.com/get-the-data/ (includes Amsterdam and North Holland)

<h2>🗄️Downloading the Dataset:</h2>

- Download Python libraries
```bash
%%capture
!pip install numpy pandas streamlit gdown currencyconverter
```
```bash
import numpy as np

# For readability purposes, scientific notation for numbers has been diabled
np.set_printoptions(suppress=True)
```
```bash
import os
import shutil

import gdown
from numpy import genfromtxt
# Download file from Google Drive
# This file is based on data from: http://insideairbnb.com/get-the-data/
file_id_1 = "13fyESiH1ZEnMV6eabAyhe20t4W6peEWK"
downloaded_file_1 = "WK1_Airbnb_Amsterdam_listings_proj.csv"

# Download the file from Google Drive
gdown.download(id=file_id_1, output=downloaded_file_1)
```
<h2>🧹Data Preprocessing:</h2>
- Inspect the csv and find the delimiter. Then output first four columns for inspection

```bash
from numpy import genfromtxt
my_data = genfromtxt(downloaded_file_1, delimiter='|', dtype="unicode")
```

```bash
my_data[:4]
```
![image](https://github.com/heyshatara/numpy-airbnb/assets/122125783/b0357b24-8cf4-4ac1-a351-94aa43a20102)

<img src="https://i.imgur.com/6am49tZ.png[/img]" height="80%" alt="Data Preprosing Results with Python 1"/>

<h2>🧹Data Cleaning:</h2>
- Remove headers, footers, redundant IDs

```bash
# Remove the first column and row
matrix = my_data[1:, 1:]
# Print out the first four columns
matrix[:, :4]
```
<img src="https://i.imgur.com/6am49tZ.png[/img]" height="80%" alt="Data Cleaning with Python Results 1"/>

- Shift dataset by 90 degrees for improved readability
```bash
# Shift the matrix by 90 degrees
matrix = matrix.T
# Print out the first five rows
# Entries: airbnb_id, price_usd, latitude, longitude
matrix[:5, :] 
```
- Filter out string characters (, and $) to perform numerical operations
```bash
# Remove the dollar sign
matrix = np.char.replace(matrix, '$', '')

# Remove the comma
matrix = np.char.replace(matrix, ',', '')
```

- Change dtype from string/Unicode to float32 to enable numerical operations
```bash
# Change Unicode to float32
matrix = matrix.astype(np.float32)
```

<h2>💱Currency Conversion + Inflation:</h2>
The Amsterdam team at Airbnb wants to provide recommendations for listings based on proximity to their destination point(s). But to make this more insightful, the rates for each listing needed to be converted to reflect the tourist's desired currency.
We created and tested this for tourists from the United Kingdom who would be booking in their GBP currencies.

- Import currency converter library
```py
from currency_converter import CurrencyConverter

cc = CurrencyConverter()

# Entries: airbnb_id, price_usd, latitude, longitude
matrix[:5, :]
```

```bash
# Get the rate of conversaton from the US dollar to GBP
gbp_rate = cc.convert(1, 'USD', 'GBP')

# Multiply the dollar column by GBP
matrix[:, 1] = matrix[:, 1]* gbp_rate
```
- We need to account for inflation which is causing Airbnb listers to raise their prices by a certain amount. In 2022, the annual inflation for GBP was 11%
```bash
# Multiply the dollar column by the inflation percentage (1.00 + inflation)
matrix[:, 1] + (1.00 + 0.11)
```
- Round results down to the nearest two decimals using NumPy
```bash
# Round down the new currency column to 2 decimals
matrix[:, 1] = np.round(matrix[:, 1], 2)

# Addtional way of rounding down the new currency colun to 2 decimals
matrix[:,1] = np.round_(matrix[:,1],decimals=2, out=None) 
```

<h2>🕹️Visualization of Findings:</h2>

- A tourist is coming from the U.K. and looking to book an Amsterdam Airbnb in their GBP currency
<img src="https://i.imgur.com/6am49tZ.png[/img]" height="80%" alt="Amsterdam Dataframe for Recommendation System"/>

- You can see this same dataset as an interactive, geographic visualization [by visiting my Streamlit](https://heyshatara-numpy-airbnb-streamlit-app-gzn89d.streamlit.app/)

- All of the Amsterdam Airbnb listings are shown in red to gauge their proximity to a specific tourist spot shown in blue, which is set as De Hooyer Windmill in Amsterdam:
<img src="https://i.imgur.com/0ECb6Yf.png" height="80%" alt="Amsterdam AirBnb Proximity Map"/>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
