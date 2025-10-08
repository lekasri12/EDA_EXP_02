# Netflix Shows & Movies
## NAME : LEKASRI G
## REGISTER NO: 212223100025

## Aim:

To analyze Netflix dataset and compare movies vs TV shows, top producing countries, and release year trends.

## Procedure / Algorithm:

  1)Load dataset (netflix_titles.csv).
  
  2)Count movies vs TV shows.
  
  3)Group by country → top contributors.
  
  4)Create pivot table (release year vs type).
  
  5)Visualize with bar & line charts.

## Program & Ouptut:
```
import pandas as pd

url = "https://raw.githubusercontent.com/allenkong221/netflix-titles-dataset/main/netflix_titles.csv"
df = pd.read_csv(url)

print("Shape:", df.shape)
print("Columns:", df.columns, "\n")
print("Type counts:\n", df['type'].value_counts(), "\n")
```

<img width="783" height="211" alt="image" src="https://github.com/user-attachments/assets/f238e77f-89a7-476a-9592-c909d1d7a7be" />

```
df['date_added'] = df['date_added'].str.strip()
df['date_added'] = pd.to_datetime(df['date_added'], errors='coerce')
df['year_added'] = df['date_added'].dt.year
df['month_added'] = df['date_added'].dt.month_name()

count_by_type = df.groupby('type')['title'].count()
print("Count by Type:\n", count_by_type, "\n")
```

<img width="302" height="118" alt="image" src="https://github.com/user-attachments/assets/47912034-c9e0-4d89-b8f6-28674c7319ab" />

```
pivot_country_type = df.pivot_table(
index='country',
columns='type',
values='title',
aggfunc='count',
fill_value=0
)

pivot_country_type['Total'] = pivot_country_type.sum(axis=1)
max_country = pivot_country_type['Total'].idxmax() # country with most titles
max_count = pivot_country_type['Total'].max() # number of titles
print("Pivot Table (Country vs Type):\n", pivot_country_type.head(), "\n")
print(f"Largest Overall: {max_country} with {max_count} titles\n")
```

<img width="668" height="210" alt="image" src="https://github.com/user-attachments/assets/84ed2a5d-03dc-4ac0-8874-a03e5a8b43b2" />

```
top_directors = df['director'].value_counts().head(5)
print("Top 5 Directors:\n", top_directors, "\n")
```
<img width="289" height="164" alt="image" src="https://github.com/user-attachments/assets/8b9c3c69-280f-4619-982a-59fe3e644150" />

```
trend = df.groupby(['year_added', 'type']).size().unstack(fill_value=0)
print("Yearly Trend by Type:\n", trend.head(), "\n")
```
<img width="284" height="170" alt="image" src="https://github.com/user-attachments/assets/4e153bf9-c55c-4f09-8e0f-e0ac9f2b3cbc" />

```
df_genre = (
df[['show_id','listed_in']]
.dropna()
.assign(listed_in=df['listed_in'].str.split(', '))
.explode('listed_in')
)

df_expanded = df.merge(df_genre, on='show_id', how='left')
print("Columns after merge:\n", df_expanded.columns)
```
```
print("\nExpanded Genre Sample:\n",
df_expanded[['title','listed_in_y']].head())
```
<img width="664" height="164" alt="image" src="https://github.com/user-attachments/assets/7e9422c6-09a3-4c7f-9624-39a1b3243edb" />

```
top_genres = df_expanded['listed_in_y'].value_counts().head(5)
print("\nTop 5 Genres:\n", top_genres, "\n")
```

<img width="336" height="185" alt="image" src="https://github.com/user-attachments/assets/5d14c60c-6d58-4ecb-9aef-c09cb9c30529" />



## Result:

Helps Netflix in content planning & investments.
