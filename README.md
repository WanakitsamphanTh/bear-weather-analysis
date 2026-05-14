# Bear Weather Data Analysis
This was originally the project in the Information Society Study (情報社会学) course during the first semester of 2025. The repository has since been updated with revised code, improved structure, and an updated dataset to include full Reiwa 7 fiscal year.
This project analyzes the relationship between bear incident reports and historical weather conditions across fiscal years Reiwa 4 – Reiwa 7 (2022–2025). Each yearly period spans from April to March of the following year.

# Table of Contents
1. [Data](##Data)
2. [Bear Incidents by Month](##Bear-Incidents-by-Month)
3. [Time Series Decomposition](##Time-Series-Decomposition)
4. [Correlation Analysis](##Correlation-Analysis)
5. [Bear Encounter Prediction with RandomForest](##Bear-Encounter-Prediction-with-RandomForest)
6. [Bear Encountering Time Series Forecasting](##Bear-Encountering-Time-Series-Forecasting)

# Data
**Historical Weahter 2022/4/1 - 2026/3/31 Data** From Japan Meteorological Agency [気象庁](https://www.data.jma.go.jp/risk/obsdl/index.php) stored in `tenki.csv` \
**Bear incident reports between 2022/4/1 - 2026/3/31 within Miyagi prefecture** : Miyagi Prefectural Government \
Reiwa 4 (2022/4/1-2023/3/31) : [令和4年度クマ目撃等情報](https://www.pref.miyagi.jp/soshiki/sizenhogo/r4kuma.html) stored in `r4koukaiyou.xlsx` \
Reiwa 5 (2023/4/1-2024/3/31) : [令和5年度クマ目撃等情報](https://www.pref.miyagi.jp/soshiki/sizenhogo/r5kuma.html) stored in `r5koukaiyou.xlsx` \
Reiwa 6 (2024/4/1-2025/3/31) : [令和6年度クマ目撃等情報](https://www.pref.miyagi.jp/soshiki/sizenhogo/r6kuma.html) stored in `r6koukaiyou.xlsx` \
Reiwa 7 (2025/4/1-2026/3/31) : [令和7年度クマ目撃等情報](https://www.pref.miyagi.jp/soshiki/sizenhogo/r7kuma.html) stored in `r7koukaiyou.xlsx` 

## Weather data
`tenki.csv` consists of the following columns
- 平均気温(℃) - Average temperature (℃)
- ~平均気温(℃) - Additional flags of average temperature [not used here]~
- 降水量の合計(mm) - Precipitation (mm)
- ~降水量の合計(mm) - Additional flags of precipitation (mm) [not used here]~
- ~降水量の合計(mm) - Additional flags of precipitation (mm) [not used here]~
- 日照時間(時間) - Sunshine hours (h)
- ~日照時間(時間) - Additional flags of sunshine hours (mm) [not used here]~
- ~日照時間(時間) - Additional flags of sunshine hours (mm) [not used here]~
- 平均風速(m/s) - Average wind speed (m/s)
- ~平均風速(m/s) - Additional flags of average wind speed [not used here]~
- 平均蒸気圧(hPa) - Average steam pressure (hPa)
- ~平均蒸気圧(hPa) - Additional flags of average steam pressure [not used here]~

## Bear incident report statistical data
Bear incidents report statistical data consists of the following data
- 番号 - Index
- 発見日時 - Date of incident (month, day, time)
- 事務所 - The station where the incident is reported
- 市区町村 - The place of incident
- 地区 - The place of incident
- 発見頭数 - The number of bears encountered
- 痕跡 - Whether the bears were encountered or only the traces found

In this project, I only use 発見日時 and transform into report date. The data are grouped by date and counted by each. Note that I ingore the places and the manner how the bears were encountered. Every report is counted as 1. The final bear incident data will be as the following: \

| 年月日 (date of incident) | 発見回数 (number of reports) |
| :--- | :--- |

## Joint data
The two data are joint on 年月日 (date). When number of reports is lacking from a row (which means there is no incident reported), it is filled by 0. The following shows columns of the table after join
| 年月日 |	発見回数 | 平均気温(℃) |	降水量の合計(mm) |	日照時間(時間) |	平均風速(m/s) |	平均蒸気圧(hPa) |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| date |	number of reports | Average temperature (℃) |	Precipitation (mm) |	Sunshine hours (h) |	Average wind speed (m/s) |	Average steam pressure (hPa) |

Some additional columns may be added:
- 目撃の有無 - 1 if the there is at leat one incident reported and 0 otherwise (for prediction)
- 1月, 2月, ... - month in which the incidents occured (for machine learning since time of the year also factors the probability)

# Bear Incidents by Month
![Monthly Incident Counts](./bear-data/monthly-count-bar.png)
Bear encounter incidents were lowest from January to March, which corresponds to the typical hibernation period.
![Monthly Incident Counts by Year](./bear-data/monthly-count-bar-by-year.png)
In Reiwa 4 and Reiwa 6, the number of bear incidents peaked during summer, whereas in Reiwa 5 and Reiwa 7, incidents were highest during autumn. This suggests the presence of a recurring two-year pattern.

# Time Series Decomposition

# Correlation Analysis

# Bear Encounter Prediction with RandomForest
## Setting 1
**Input** \
All data are standardized. \
**Output** : the probability of bear encountering 1 or 0\
**Model**: RandomForestClassifier with 80 estimators and max_depth = 5. \
**Train-test split**: Data prior to Reiwa 7 fiscal year is the train set and Reiwa 7 fiscal year data the test set.

## Results


# Bear Encountering Time Series Forecasting
## Settings
**Input**: 
- 平均気温(℃) (Average temperature)
- 平均風速(m/s) (Average wind speed)
- 月 (month)

**Output**: the number of bear reports to be expected \
Both input and output data are standardized with mean=0.0 and range [-1,1]. The scaler is fit on the train set (Reiwa 4-6). 

**Model Structure**

**Parameters**: \
`hidden_dim` = 128 : the dimension of hidden state vectors \
`layer_dim` = 1 : the number of recurrent layer \
`lr` = 1e-3 : learning rate \
`epochs` = 150

## Results
