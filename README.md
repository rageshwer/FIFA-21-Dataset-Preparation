The dataset used in this project is a FIFA 2021 player statistics dataset containing
attributes of professional football players. The dataset consists of:<br>
• Total Records (Rows): 18,979 players<br>
• Total Attributes (Columns): 77 features<br>
<br>
Out of these: 54 attributes are numerical and 23 attributes are categorical (including mixed-
type columns like Hits). The target variable for this analysis is: OVA (Overall Rating)<br>
<br>
# Types of Attributes :<br>
The dataset includes continuous numerical data (height, weight, value, wage), discrete
numerical data (ratings, skill scores), and categorical data (nominal: club, nationality,
preferred foot and ordinal: attacking work rate, defensive work rate, skill moves, weak foot,
IR).<br>
<br>
# Initial Issues Identified :<br>
Some numerical features, like Height, Weight, and Value, were stored as strings.
Units were inconsistent, such as lbs vs kg and M vs K. Columns like Hits and Contract had
symbols in them. These columns were cleaned early so that data attributes can be classified
easily.<br>
<br>
# Irrelevant Features Identified:<br>
The following columns were considered irrelevant for predicting OVA:<br>
• Name<br>
• LongName<br>
• photoUrl<br>
• playerUrl<br>
• Joined<br>
• ID<br>
<br>
# Outlier Analysis:<br>
Outliers were detected using the Interquartile Range (IQR) method.
Columns with significant outliers:<br>
• Value<br>
• Wage<br>
• Hits<br>
• Goalkeeping attributes<br>
<br>
### Reason for outliers:<br>
• Player popularity (Hits), some players are far more popular.<br>
• Elite players (Value, Wage), top players are paid a huge sum of money that seems like
outliers.<br>
• Position-specific attributes (Goalkeepers vs field players), some features provide high
ranking to only goalkeepers which are lesser in number.<br>
Outliers were not removed, as they represent real-world variation.
<br>
# Preprocessing Steps<br>
## Data Type Corrections<br>
Several columns were converted into proper numeric formats:<br>
• Height: Converted from feet/inches → cm<br>
• Weight: Converted from lbs → kg<br>
• Value, Wage, Release Clause: Converted from €K/€M → numeric<br>
• Hits: Converted from string (K format) → integer<br>
<br>
## Handling Missing Values<br>
• Loan Date End: This feature only has loan end date for players on loan. This is same as
contract end date. So,
converted into a new binary feature and original column was removed:<br>
▪ 1 → On Loan<br>
▪ 0 → Not on Loan<br>
<br>
## Feature Transformation<br>
• Positions: Instead of information about what positions, it was converted to how many
positions a player is capable of playing. This numeric data is helpful as higher the
number, higher is the player’s versatility.<br>
<br>
Min-Max Scaling (Range: 1–100) applied to:<br>
◦ Value<br>
◦ Wage<br>
◦ Release Clause<br>
Reason:These features had very large ranges and could dominate the model.<br>
## Feature Engineering<br>
New features created:<br>
• BMI<br>
• Total Work = A/W + D/W<br>
• Attack Score = sum of attacking attributes<br>
• Defence Score = sum of defensive attributes<br>
• On Loan (binary feature)<br>
<br>
# Final Data Summary<br>
After preprocessing: Dataset has 75 features with no missing values. All inconsistencies have
been resolved with new features also being added.<br>
• Dataset split:<br>
◦ 80% Training<br>
◦ 20% Testing<br>
<br>
Splitting ensures that the model is training on data with some set of observations and
testing on those which are new to it. This avoids model to learn metrics about test data
and prevents any data leakage and inflated results.<br>
After the preparation of data, a linear regression model trained on the data gave a R_Sq score of 0.98 and MAE : 0.61. This is not due to the overfitting as the test score is also the same.
The features have very high VIF scores showing multicollinearity. This is because the
feature values are highly linked with each other and can be easily derived. For the sake
of prediction these features are kept.
