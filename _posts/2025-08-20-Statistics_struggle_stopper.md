---
published: true
math: true
title: Statistics struggle stopper - foundation
date: 2025-08-20 15:00:00 -0300
categories: [Python, statistics]
tags: [python, scikit-learn, statistics, basics]     # TAG names should always be lowercase

description: Notes to help wrap my head around statistics.
---

## Data Analysis with Python

In my data journey next steps, I decided to explore education possibilities from more "advanced" platforms, so looking for references from people with a similar mindset sounded like a good plan. But people don't always speak volumes online (myself included!), so finding that was an easy task. Luckily I stumbled upon [Mr. Wolfgang Huang](https://www.wolfgang-huang.de/) and his very well made [Nobel laureates dashboard](https://www.nbldata.org/list). Browsing through Mr. Huang's pages online, I noticed he took the IBM Data Science Specialization full course (made of 12 courses) on Coursera and decided to understand more about this.

At the moment of this post, I am at the 7th of 11 courses of the IBM Data Analyst Specialization course... and now the struggle hit me real hard.

This post is an attempt to consolidate the **Statistics** course module content. The data analyst workbench may be made of a variety of resources, data may come from many sources, and also have many shapes and adjustments may be necessary, but the starting point for these notes will be a fairly well-behaved table.

---

## Workbench

To deal statistically with a "real world dataset", it is expected we perform cleaning and transformations on it. Datasets come with "unsuitabilities" that stand in the way of their ability to provide insights. Some columns need to be **fixed** (*data is miswritten or does not exist*), others need to be **enriched** (*data benefits from improvement*) and somtimes we'll even **generate** data (*data simply is not there yet!*) based on what is given to us because, in the end, chances are we will end up drarwing a plot to let us see what the data is trying to show.

### 1. Data normalization (enrichment action)

Datasets contain numerical information measured in different magnitudes (a person *age* will never surpass *three digits*, but it's different for *distances*, or *currency*). To give data fair and meaningful statistic treatment, we should scale the data among them so their magnitudes don't distort the results of the calculations that will be applied.

In this example dataset with the following columns:

| first_name   | last_name     | gender   |   age |   salary |
|:-------------|:--------------|:---------|------:|---------:|
| Damon        | Christescu    | Male     |    28 |   406877 |
| Kev          | Nisuis        | Male     |    57 |   373312 |
| Tedi         | Start         | Female   |    66 |   329638 |
| Nancee       | Van der Kruis | Female   |    37 |   207422 |
| Wilton       | Kirsche       | Male     |    22 |   206600 |

#### 1.1. Simple feature scaling approach

In this intuitive approach we scale (divide by) all the values of a dimension according to the maximum value found.

#### 1.2. "Min-max" approach

Just like scaling, but also considers the minimum and maximum values found in each dimension. Each normalized value $$x$$ will be replaced by a normalized version $$x_{norm}$$ like so:

$$
\begin{equation}
    x_{norm} = \frac{x - x_{min}}{x_{max}-x_{min}}
    \label{eq:min_max}
\end{equation}
$$

For the column *age*, assuming df is already assigned to a variable:

```python
df["normalized_age"] = (df["age"]-df["age"].min())/(df["age"].max()-df["age"].min())
df["normalized_salary"] = (df["salary"]-df["salary"].min())/(df["salary"].max()-df["salary"].min())

```

The imaginary dataset assigned to a Python variable **df** below illustrates it:

| first_name   | last_name     | gender   |   age |   salary |   normalized_age |   normalized_salary |
|:-------------|:--------------|:---------|------:|---------:|-----------------:|--------------------:|
| Damon        | Christescu    | Male     |    28 |   406877 |        0.192308  |            0.816438 |
| Kev          | Nisuis        | Male     |    57 |   373312 |        0.75      |            0.748532 |
| Tedi         | Start         | Female   |    66 |   329638 |        0.923077  |            0.660174 |
| Nancee       | Van der Kruis | Female   |    37 |   207422 |        0.365385  |            0.412915 |
| Wilton       | Kirsche       | Male     |    22 |   206600 |        0.0769231 |            0.411251 |


#### 1.3. *Z-score* normalization approach

If the data distribution behaves **normally**, each value can be normalized by it's Z equivalent according to the calculation:

$$
\begin{equation}
    x_{norm} = \frac{x - x_{avg}}{\sigma}
    \label{eq:znorm}
\end{equation}
$$

Where $$\sigma$$ is the *standard deviation* calculated for the column. Again, for the column "Age":

```python
df["Z Normalized Age"] = 
(df["Age"]-df["Age"].avg())
/
df["Age"].std()
```

### 2. Binning (generation action)

Binning consists of creating classes that describe ranges of values. Ages can be coerced into classes like *Young*, *Adult* or *Old*. In Python we could achieve that with Numpy's `linspace()` like so:

```python
# Since THREE bins require FOUR dividers:
bins = np.linspace(min(df["age"]), max(df["age"]), 4)
# A list with the groups names:
group_names = ["Young", "Adult", "Old"]
# Finally pd.cut() assigns the bins their names:
df["binned_age"] = pd.cut(df["age"], bins, labels=group_names, include_lowest=True)
# To count the records in each bin:
df["binned_age"].value_counts()

```

| binned_age   |   count |
|:-------------|--------:|
| Old          |      35 |
| Young        |      33 |
| Adult        |      32 |


### 3. Grouping and summarizing (addition action)

The grouping methods let us make sense of information regarding bins. 

#### 3.1. Groupby()

This pandas method summarizes data keeping it in a tabular structure that can be easily used in visualizations. As an example, the same summary as `df["binned_age"].value_counts()` provides using `groupby()`:

```python
df_grp = df[["binned_age","age"]]
dfinal=df_grp.groupby(["binned_age"], as_index=False).count()
```

Or we can introduce more layers of information, allowing us to differentiate the count among genders:

```python
df_grp = df[["gender","binned_age","age"]]
dfinal=df_grp.groupby(["gender","binned_age"], as_index=False).count()
```

| gender   | binned_age   |   age |
|:---------|:-------------|------:|
| Female   | Young        |    16 |
| Female   | Adult        |    17 |
| Female   | Old          |    16 |
| Male     | Young        |    17 |
| Male     | Adult        |    15 |
| Male     | Old          |    19 |

The `groupby()` method expects you to aggregate data somehow; count is the most basic. There's also `sum()`, `avg()`, `std()` for standard deviation and some others. Despite being basic (or maybe for that reason) these operations are pretty useful to gain insights on the dataset.

#### 3.2. Pivot tables

If a more human readable form is necessary, we can probably use a pivot table. This table conveys the same information as the table above, it is just more readable (which is not necessarily computationally interesting)

```python
df_pivot = dfinal.pivot(index="gender", columns="binned_age").reset_index()
```

| gender           |  age count 'Young' |  age count 'Adult' |  age count 'Old' |
|:-----------------|-------------------:|-------------------:|-----------------:|
| Female           |                 16 |                 17 |               16 |
| Male             |                 17 |                 15 |               19 |

---

That's it for the basics, which encompasses some very popular Excel basic functions.

Next I will approach basic Statistics relationships and hypothesis tests and start to use Math the proper way to challenge our statistic intuition, well renowed in leading us into misguided decisions.
