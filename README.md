# LoL Esports Matches Win Rate Analysis
by Andrew Zhao (yiz158@ucsd.edu)

Yiheng Yuan (yiy159@ucsd.edu)

___
## Introduction
- Our data contains the information of **LoL** (League of Legends, a popular moba games) **esports matches** in **2022**.
 - The data is taken from [this website](https://oracleselixir.com/tools/downloads)
 - The question our analysis investigates on is 
 : 

 *Is `Jinx` more Likely to Win than `Aphelios` at any Given Match in 2022?*

 - This question is crucial for understanding the **mechanism** of picking characters in LoL professional esports games and the **overall design/fairness** of the game.
 - Here are some of the general statistics about our data:
    - The orignial dataset contains `149232 rows` and `123 columns`
    - The columns that are relevant to our data: 
        - `patch` (Float): contains the information about patch ( patch is similar to a client version. [Click here to read more](https://leagueoflegends.fandom.com/wiki/Patch_(League_of_Legends)#:~:text=A%20patch%20(otherwise%20known%20as,the%20power%20balance%20between%20champions.)). (**!!!Note: We ended up not using this column in our hypothesis test but chose to keep it as it makes our analysis part more smooth**)
         - `date` (String): contains the information of the date that a particular match is played.
         - `champion` (String): contains information about the champion that the player picked in a particular game.
         - `result` (Integer): contains information about whether a game is won or lost.

___
## Cleaning and EDA

**Data Cleaning steps**:

1. Clean out the `position` that equals to **team** so we are only looking at the data of **players**
2. We **extract** the **columns** that are needed for furthur investigation
``` 
['patch', 'datacompleteness', 'champion', 'result']
```
3. We observe that `patch` has **missing values** and through our research we found that `patch` is related to `date`. [Click here to read the official source](https://leagueoflegends.fandom.com/wiki/Patch_(League_of_Legends)#:~:text=A%20patch%20(otherwise%20known%20as,the%20power%20balance%20between%20champions.)). If the `date` is in a certain range then the missing patch must be the **same** as other games in this range. 

4. Based on the observation from **step3**, we imputed the `patch` column based on `date`. 

5. Now we want to change the **type** of some of the **columns**. The `date` column is now stored as **string** but it makes more sense to have it as **datetime** object. The `result` column is stored as **int64** and we could save memeory if we change it to **bool**

**Here is the first few rows of our cleaned dataframe:**

|   patch | date                | champion   | result   |
|--------:|:--------------------|:-----------|:---------|
|   12.01 | 2022-01-10 07:44:08 | Jinx       | True     |
|   12.01 | 2022-01-10 09:24:26 | Jinx       | True     |
|   12.01 | 2022-01-10 09:24:26 | Aphelios   | False    |
|   12.01 | 2022-01-10 09:51:16 | Aphelios   | True     |
|   12.01 | 2022-01-10 10:09:22 | Jinx       | True     |

___
**Univariate Analysis**

<iframe src="Assets/champ.html" width=600 height=600 frameBorder=0></iframe>

**Exaplantion**:
Based on histogramof pick rate between Jinx and Aphelios, we observed Jinx has a lower pick rate than Aphelios over the entire dataset.  

___
**Bivariate Analysis**

<iframe src="Assets/date_character.html" width=600 height=600 frameBorder=0></iframe>

**Exaplantion**:
We observe that the number of games each chacter appears is distributed differently throughout the year. Some date intervals have more games that have `Jinx` and `Aphelios` and some contain less. For example `February` has significantly more games that contain `Jinx` and `Aphelios` than `December`. Another observation is that in some month `Aphelios` appeared in more games than `Jinx` and vise versa. 

<iframe src="Assets/result_champ.html" width=600 height=600 frameBorder=0></iframe>

**Exaplantion**:
We observe that `Jinx` has a slightly higher **overall** win rate than `Aphelios` at any given match. This observation leads us to think: **Is Jinx more Likely to Win than Aphelios at any Given Match?** which is essentially what the anlysis is about. 

___
**Interesting Aggregates**

|   patch | champion   |   result |
|--------:|:-----------|---------:|
|   12.01 | Aphelios   | 0.49375  |
|   12.01 | Jinx       | 0.542453 |
|   12.02 | Aphelios   | 0.478723 |
|   12.02 | Jinx       | 0.505455 |
|   12.03 | Aphelios   | 0.473002 |
|   12.03 | Jinx       | 0.530055 |
|   12.04 | Aphelios   | 0.480328 |
|   12.04 | Jinx       | 0.537805 |
|   12.05 | Aphelios   | 0.495475 |
|   12.05 | Jinx       | 0.5      |
|   12.06 | Aphelios   | 0.5      |
|   12.06 | Jinx       | 0.521739 |
|   12.07 | Aphelios   | 0.6875   |
|   12.07 | Jinx       | 0.575    |
|   12.08 | Aphelios   | 0.352941 |
|   12.08 | Jinx       | 0.333333 |
|   12.09 | Aphelios   | 0.434783 |
|   12.09 | Jinx       | 0.432432 |
|   12.1  | Aphelios   | 0.6      |
|   12.1  | Jinx       | 0.478992 |
|   12.11 | Aphelios   | 0.568047 |
|   12.11 | Jinx       | 0.398438 |
|   12.12 | Aphelios   | 0.508642 |
|   12.12 | Jinx       | 0.395833 |
|   12.13 | Aphelios   | 0.478022 |
|   12.13 | Jinx       | 0.48     |
|   12.14 | Aphelios   | 0.459184 |
|   12.14 | Jinx       | 0.387755 |
|   12.15 | Aphelios   | 0.57265  |
|   12.15 | Jinx       | 0.428571 |
|   12.16 | Aphelios   | 0.433962 |
|   12.16 | Jinx       | 0.5      |
|   12.17 | Aphelios   | 1        |
|   12.17 | Jinx       | 0.333333 |
|   12.18 | Aphelios   | 0.526316 |
|   12.18 | Jinx       | 0.346154 |
|   12.19 | Aphelios   | 0.363636 |
|   12.19 | Jinx       | 0.545455 |
|   12.2  | Aphelios   | 0.46875  |
|   12.2  | Jinx       | 0.4      |
|   12.21 | Aphelios   | 0.4375   |
|   12.21 | Jinx       | 0.2      |
|   12.23 | Aphelios   | 0.25     |
|   12.23 | Jinx       | 1        |

**Exaplantion**:
This grouped table shows the win rate of `Jinx` and `Aphelios` individually by `patch`. Though this is not closely related to our anlysis question, however, this leads us to think of an interesting question: **Does patch influence the win rate difference between Jinx and Aphelios?**. Some idea we have in mind would be performing an `ANOVA` F-test, though we are not going to explore this topic on this project.

___
## Assessment of Missingness

___
**NMAR Analysis**
 - We believe the **missingness** in the `monsterkillsownjungle` and `monsterkillsenemyjungle` columns is **NMAR**. 
    - First, it is not **MD** and **MAR** because its missingness **does not** depend on any other column in the dataset. We have investigated all possible columns, such as `datacompleteness`, `date`, `league`, etc., that could be related. Still, none of them seems to have a relationship with the missingness, meaning we can find the missingness **evenly distribute in all other columns**. Because of this, we cannot determine the missing value precisely by looking at the other columns. 
    - Second, **there is a good reason why the missingness depends on the values themselves**. After our investigation from the `league` column on the missing value of `monsterkillsownjungle` and `monsterkillsenemyjungle` columns, we find that some leagues, such as `LDL` and `LPL`, are doing a great job of recording the value for `monsterkillsownjungle` and `monsterkillsenemyjungle` columns. However, some matches **still** miss the values of `monsterkillsownjungle` and `monsterkillsenemyjungle`. We think the value for the `monsterkillsownjungle` and `monsterkillsenemyjungle` columns **may be recorded depending on where the match happens**. And in some locations (since matches in a region happens may happens in different cities, [read more about it here](https://liquipedia.net/leagueoflegends/LPL/2023/Spring)). Some of them may not have the rule to record this kind of data. We may explain the missingness if we collect all matches’ locations (cities) or rubrics for recording monster kills in their jungle and enemy jungle for each player.
    - Lastly, it is not **MCAR** because it may depend on the values themselves, such as some matches may have different rubrics for recording specific data.

___
**Missingness Dependency**

**Missingness Dependency Investigation Question (does depends on):**
*Does Missingness in `patch` column  depend on month of `date` column?*

Our **null hypothesis** is that the distribution of the `month` where the `patch` is missing is same as the distribution of the `month` where the `patch` is not missing.
**Alternative hypothesis** is that the distribution of the `month` where the patch is missing is not the same as the distribution of the `month` where the patch is not missing.

**Investigation:**
After investigating the dataset, we find that the `patch` column has **108** missing values over all 149232 values, and the missingness **may** depend on the month of the `date` column. So, we want to identify whether the missingness depends on the month of the `date` column.(**!!!Note: we used the month instead of the whole datetime is because all the time is within the same year and it will be easier to compare within each month rather than each different time from days. And if we find there is a relationship between month and missingness of patch, we can also conclude that there is a relationship between date and missingness of patch**).


**Graph Shows the Distribution of Missingness by `Month`**

<iframe src="Assets/month_missngness.html" width=600 height=600 frameBorder=0></iframe>

We use the **TVD test** to test the null hypothesis and find out the **p-value** is close to 0.0 under 500 repetitions. 

<iframe src="Assets/tvd.html" width=600 height=600 frameBorder=0></iframe>

**Result:**
We reject our null hypothesis that the missingness in `patch` column does depend on month of  `date` column. Additionally, the missingness is not MD because we cannot exactly predict the missingness from `date`. (Note: there are still patch values that are not missing and is from the same month where the patch is missing).

___
**Missingness Dependency Investigation Question (does not depends on):**
*Does Missingness in `patch` column  depend on month of `result` column?* 

Our **null hypothesis** is that the distribution of the `result` where the `patch` is missing is same as the distribution of the `result` where the `patch` is not missing.
**Alternative hypothesis** is that the distribution of the `result` where the `patch` is missing is not the same as the distribution of the `result` where the `patch` is not missing.

**Investigation:**
After investigating the dataset, we find that the `patch` column has **108** missing values over all 149232 values, and the missingness **may** depend on the `result` column. So, we want to identify whether the missingness depends on thethe `result` column.


**Graph Shows the Distribution of Missingness by `Result`**


<iframe src="Assets/missing_result.html" width=600 height=600 frameBorder=0></iframe>

We use the **TVD test** to test the null hypothesis and find out the **p-value** is close to 1 under 500 repetitions. **Note: p-value of 1.0 may seems unnormal but that is because our dataset is so large that the result contain in it is likely to be half win and half lose, and it is evenly distributed. So that by random suffling the `patch` column, the result will still likely to be half win and half lose. As randomness is also evenly distributed. And our observed tvd is so small, close to 0, due to the similarity of the distribution between result on patch is missing and result on patch is not missing.**


<iframe src="Assets/doesnot.html" width=600 height=600 frameBorder=0></iframe>

**Result:**
We fail to reject our null hypothesis so that we can not conclude the missingness in the `patch` column depends on `result` column.

___
## Hypothesis Testing
**Null Hypothesis:**
In the population, win rate of `Jinx` and `Aphelios` at any given match have the same distribution, and the observed differences in our samples are due to random chance.

**Alternative Hypothesis:**
In the population, `Jinx` has higher win rate than `Aphelios` at any given match, on average. The observed difference in our samples cannot be explained by random chance alone. 

*(Justification:)* we chose to frame our null and alternative hypothesis in this way because we observed that `Jinx` has a slightly higher overall win rate than `Aphelios` so we want to test whether this observation was due to a random chance. 

**Test Statistics:**
TVD (in a Permutation test)

*(Justification:)* we chose to use permutation test because we want to test if the win rate of these two champions, which are two samples, are from the same distribution. We chose to use TVD as our test statiscs because `champion` and `result` are categorical. 

**Significance Level:**
0.05

*(Justification:)* by convention. 


**Result P-value:**
0.141

*(Explanation:)*  the probability, when the null hypothesis is true, the TVD result from permutation would be equal to or more extreme than the actual observed results

**Conclusion:**
Since our p-value is higher than significance level, 0.05, we fail reject our null hypothesis that in the population, win rate of `Jinx` and `Aphelios` at any given match in 2022 have the same distribution, and the observed differences in our samples are due to random chance at a significance level of 0.05.