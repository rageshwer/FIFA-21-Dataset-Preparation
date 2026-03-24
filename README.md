A) Dataset Description
The dataset used in this assignment is a FIFA 2021 player statistics dataset containing detailed attributes of professional football players. The dataset consists of:
Total Records (Rows): 18,979 players
Total Attributes (Columns): 77 features

Out of these: 54 attributes are numerical and 23 attributes are categorical (including mixed-type columns like Hits)
The target variable for this analysis is: OVA (Overall Rating)
Types of Attributes :
The dataset includes a mix of continuous numerical data (height, weight, value, wage), discrete numerical data (ratings, skill scores), and categorical data (nominal: club, nationality, preferred foot and ordinal: attacking work rate, defensive work rate, skill moves, weak foot, international reputation).

















Initial Issues Identified :
Some numerical features, like Height, Weight, and Value, were stored as strings.                   Units were inconsistent, such as lbs vs kg and M vs K. 					Mixed-format columns, like Hits and Contract, were also present. 				Early conversion and cleaning were performed to address these issues, correcting inconsistencies in the columns.
B) Data Quality Analysis
Missing Values:
Only two columns contained missing values:						
Loan Date End
Hits
Loan Date End: 17966
Hits: 2595
Visualisation using a heat-map confirmed the distribution of missing values.

Duplicate Records:										No duplicate rows were found in the dataset.
Noise and Inconsistencies:								Noise in the dataset was identified in the form of:
Mixed units (Height, Weight)
Currency formats (€, M, K)
Star ratings with symbols (★)
Mixed data types (Hits column)

Irrelevant Features Identified:								The following columns were considered irrelevant for predicting OVA:
Name
LongName
photoUrl
playerUrl
Joined
ID

Outlier Analysis:
Outliers were detected using the Interquartile Range (IQR) method.
Columns with significant outliers:
Value
Wage
Hits
Goalkeeping attributes

Reason for outliers:
Player popularity (Hits), some players are far more popular.
Elite players (Value, Wage), top players are paid a huge sum of money that seems like outliers.
Position-specific attributes (Goalkeepers vs field players), some features provide high ranking to only goalkeepers which are lesser in number.
Outliers were not removed, as they represent real-world variation.
C) Preprocessing Steps
Data Type Corrections
Several columns were converted into proper numeric formats:
Height: Converted from feet/inches → cm
Weight: Converted from lbs → kg
Value, Wage, Release Clause: Converted from €K/€M → numeric
Hits: Converted from string (K format) → integer

Handling Missing Values
Loan Date End: This feature only has loan end date for players on loan. This is same as contract end date. So,
Converted into a new binary feature:
1 → On Loan
0 → Not on Loan
Original column removed.
Hits:
Missing values replaced with mean value
Justification: No strong correlation with other features.

Handling Inconsistencies
Contract column: Converted into contract end year. “Free” mapped to 0

Encoding Categorical Variables
Label Encoding applied to: Since these columns had high number of unique values, label encoding was better as compared to one hot encoding.
Best Position
Nationality
Club
Binary Mapping:
Preferred Foot → (Left = 0, Right = 1)
Ordinal Encoding:
A/W, D/W → Low (0), Medium (1), High (2)
Star Ratings (IR, SM, W/F):
Removed “★” symbol and converted to integers
Feature Transformation
Positions: Instead of information about what positions, it was converted to how many positions a player is capable of playing. This numeric data is helpful as higher the number, higher is the player’s versatility.
Converted from string → count of positions
Feature Scaling
Min-Max Scaling (Range: 1–100) applied to:
Value
Wage
Release Clause
Reason:
These features had very large ranges and could dominate the model.
Feature Engineering
New features created:
BMI
Total Work = A/W + D/W
Attack Score = sum of attacking attributes
Defence Score = sum of defensive attributes
On Loan (binary feature)

Feature Selection
Removed irrelevant columns:
ID, Name, LongName, photoUrl, playerUrl, Joined

D) Justification for preprocessing decisions
Each preprocessing step was carefully chosen:
Unit Standardisation (Height, Weight): Ensures consistency and comparability across records
Currency Conversion: Required for numerical operations and scaling
Handling Missing Values:
Binary transformation (Loan Date) preserves information
Mean imputation (Hits) avoids data loss
Encoding Techniques:
Label encoding reduces dimensionality
Avoided one-hot encoding due to high cardinality
Scaling: Prevents bias in ML models due to large-value features
Feature Engineering: Improves predictive power by combining meaningful attributes
Outlier Retention: Important because extreme values reflect real player differences

E) Final Data Summary
After preprocessing:
Dataset became fully numerical and ML-ready
No missing values remain
All inconsistencies resolved
New meaningful features added
Final Steps Performed
Target variable: OVA
Dataset split:
80% Training
20% Testing
Model Applied
Linear Regression
Evaluation Metric
Mean Absolute Error (MAE)
The model was successfully trained on the cleaned dataset, demonstrating that preprocessing prepared the data effectively for machine learning tasks
The MAE score = 0.62
