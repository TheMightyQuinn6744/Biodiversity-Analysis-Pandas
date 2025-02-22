Capstone 2: Biodiversity Project

Introduction

You are a biodiversity analyst working for the National Parks Service. This project involves analyzing species data from various national parks to gain insights into biodiversity and conservation efforts.

Note: The data used in this project is inspired by real-world data but is largely fictional.

Data and Setup

Required Libraries

The following libraries are required to execute the project:

from matplotlib import pyplot as plt
import pandas as pd
from scipy.stats import chi2_contingency

Data Files

species_info.csv: Contains information about species, including:

scientific_name: Scientific name of each species.

common_names: Common names of each species.

conservation_status: Conservation status of each species.

observations.csv: Contains observation counts of different species across national parks.

Data Loading

species = pd.read_csv('species_info.csv')
observations = pd.read_csv('observations.csv')

Data Exploration

Inspecting the Data

print(species.head())
print(species.nunique())

Unique Categories and Conservation Status

print(species.category.unique())
print(species.conservation_status.unique())

Data Cleaning and Analysis

Handling Missing Values

species.fillna('No Intervention', inplace=True)

Conservation Status Analysis

Using groupby to count species per conservation status:

protection_counts = species.groupby('conservation_status')\
    .scientific_name.nunique().reset_index()\
    .sort_values(by='scientific_name')

Visualizing Conservation Status by Species

plt.figure(figsize=(10, 4))
ax = plt.subplot()
plt.bar(range(len(protection_counts)), protection_counts.scientific_name.values)
ax.set_xticks(range(len(protection_counts)))
ax.set_xticklabels(protection_counts.conservation_status.values)
plt.ylabel('Number of Species')
plt.title('Conservation Status by Species')
plt.show()

Species Protection Analysis

Creating is_protected Column

species['is_protected'] = species.conservation_status != 'No Intervention'

Grouping by Category and Protection Status

category_counts = species.groupby(['category', 'is_protected'])\
                         .scientific_name.nunique().reset_index()

Pivoting Data

category_pivot = category_counts.pivot(columns='is_protected',
                                       index='category',
                                       values='scientific_name')\
                                 .reset_index()
category_pivot.columns = ['category', 'not_protected', 'protected']

Calculating Percentage of Protected Species

category_pivot['percent_protected'] = category_pivot.protected / \
                                      (category_pivot.not_protected + category_pivot.protected)

Statistical Analysis

Chi-Squared Tests for Significance

contingency1 = [[30, 146], [75, 413]]
chi2_contingency(contingency1)

contingency2 = [[30, 146], [5, 73]]
chi2_contingency(contingency2)

Observing Sheep Species

Identifying Sheep Species

species['is_sheep'] = species.common_names.apply(lambda x: 'Sheep' in x)
sheep_species = species[(species.is_sheep) & (species.category == 'Mammal')]

Merging with Observations Data

sheep_observations = observations.merge(sheep_species)

Total Sheep Observations per Park

obs_by_park = sheep_observations.groupby('park_name').observations.sum().reset_index()

Visualizing Sheep Observations

plt.figure(figsize=(16, 4))
ax = plt.subplot()
plt.bar(range(len(obs_by_park)), obs_by_park.observations.values)
ax.set_xticks(range(len(obs_by_park)))
ax.set_xticklabels(obs_by_park.park_name.values)
plt.ylabel('Number of Observations')
plt.title('Observations of Sheep per Week')
plt.show()

Foot and Mouth Disease Analysis

Sample Size Calculation

minimum_detectable_effect = 100 * 0.05 / 0.15

Weeks of Observation Needed

bryce_weeks = 870 / 250.
yellowstone_weeks = 810 / 507.

Conclusion

This project provides an in-depth analysis of biodiversity data, conservation efforts, and species observations across national parks. The statistical tests help determine the significance of species protection efforts, and observation data assists in tracking species health.

Future Work

Further refine the analysis using additional datasets.

Implement machine learning models for species classification.

Conduct in-depth studies on disease spread and its impact on biodiversity.

