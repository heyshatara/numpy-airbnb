# Amsterdam AirBnB Recommendations System
<img src="https://imgur.com/vr42hjE" height="50%" alt="Amsterdam's De Gooyer Windmill"/>
Acting as the Lead Data Scientist for the Amsterdam area at Airbnb, we'll analyze real data from different listings along with related data, and make decisions on how we can structure our data.
The resulting dataset will then be used to develop a recommendation system to enable tourists to identify the best AirBnB listing based on proximity to locations they plan to visit in Amsterdam.

<h2>Project Description</h2>
In this data science portfolio project, I utilized Python and NumPy to clean and analyze a dataset containing booking information for AirBnB locations in Amsterdam. The objective of this project was to clean the dataset, convert its currency and aggregate calculations. The resulting dataset was then used to develop a recommendation system to enable tourists to identify the best AirBnB listing based on proximity to locations they planned to visit in Amsterdam.
<br />

<h2>Languages/Libraries and Utilities Used</h2>

- <b>Python (NumPy)</b> 
- <b>Streamlit</b>


<h2>Project Overview:</h2>
The AirBnB Amsterdam Recommendation System project showcases the application of Python, NumPy, and data science techniques to derive meaningful recommendations for AirBnB accommodations in Amsterdam, ultimately assisting users in finding the most suitable listings for their selected tourist location(s).

<h2>Dataset Description:</h2>
Amsterdam subset of data from: http://insideairbnb.com/get-the-data/ (includes Amsterdam, North Holland)

<h2>Data Cleaning and Preprocessing:</h2>

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
```

- Remove headers, footers, redundant IDs
```bash
# Remove the first column and row
matrix = my_data[1:, 1:]
```

- Shift dataset by 90 degrees for improved readability
```bash
# Shift the matrix by 90 degrees
matrix = matrix.T
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

<h2>Data Analysis and Insights:</h2>
The Amsterdam team at Airbnb wants to provide recommendations for listings based on proximity to their destination point(s). But to make this more insightful, the rates for each listing needed to be converted to reflect the tourist's desired currency.
We created and tested this features for tourists from the United Kingdom would would be booking in their GBP currencies.

## Import currency converter library

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

<h2>Recommendation System Approach:</h2>
The AirBnB Amsterdam Recommendation System is a data science portfolio project that aims to provide users with personalized recommendations for the best AirBnB listings based on their planned visit locations in Amsterdam. By analyzing a dataset containing booking information for AirBnB locations in Amsterdam, this project utilizes Python and NumPy for data cleaning and analysis.

<h2>Visualization of Findings:</h2>
You can see the dataset as a Dataframe and as an interactive, geographic visualization [by visiting Streamlit](https://heyshatara-numpy-airbnb-streamlit-app-gzn89d.streamlit.app/)
All of the Amsterdam Airbnb listings are shown in red to gauge their proximity to a specific tourist spot shown in blue:
<img src="https://i.imgur.com/0ECb6Yf.png" height="80%" alt="Amsterdam AirBnb Proximity Map"/>
<h2>Documentation and Report:</h2>
## Download Python libraries
<h2>Conclusion and Key Takeaways:</h2>
<br />
<br />
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
