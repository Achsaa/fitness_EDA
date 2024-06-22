# fitness_EDA
# Running Activities EDA

This repository contains an exploratory data analysis (EDA) for running activities data. The dataset includes various details about running activities such as distance, duration, average pace, average speed, calories burned, climb, and average heart rate.

## Dataset

The dataset (`cardioActivities.csv`) should include the following columns:
- Date
- Activity Id
- Type
- Route Name
- Distance (km)
- Duration
- Average Pace
- Average Speed (km/h)
- Calories Burned
- Climb (m)
- Average Heart Rate (bpm)
- Friend's Tagged
- Notes
- GPX File

## Getting Started

1. Clone the repository:
    git clone https://github.com/yourusername/your-repo.git
    cd your-repo
   
3. Install the required Python packages:
    pip install pandas matplotlib statsmodels

4. Place the `cardioActivities.csv` file in the repository directory.

5. Run the EDA script:

    ```python
    import pandas as pd
    import matplotlib.pyplot as plt
    import statsmodels.api as sm

    # Load the CSV file into a dataframe with date parsing and setting index to Date column
    df_activities = pd.read_csv('cardioActivities.csv', parse_dates=['Date'], index_col='Date')

    # List of columns to drop
    cols_to_drop = ['Activity Id', 'Route Name', "Friend's Tagged", 'Notes', 'GPX File']

    # Drop unnecessary columns if they exist in the dataframe
    df_activities.drop(columns=[col for col in cols_to_drop if col in df_activities.columns], inplace=True)

    # Calculate the activity type counts
    activity_type_counts = df_activities['Type'].value_counts()
    print("Activity Type Counts:")
    print(activity_type_counts)

    # Rename 'Other' values to 'Unicycling' in the Type column
    df_activities['Type'] = df_activities['Type'].str.replace('Other', 'Unicycling')

    # Count the missing values in each column
    missing_values_count = df_activities.isnull().sum()
    print("\nMissing Values Count:")
    print(missing_values_count)

    # Calculate the sample mean for Average Heart Rate (bpm) for the 'Cycling' activity type
    avg_hr_cycle = df_activities[df_activities['Type'] == 'Cycling']['Average Heart Rate (bpm)'].mean()

    # Filter the df_activities for the 'Cycling' activity type and create a copy
    df_cycle = df_activities[df_activities['Type'] == 'Cycling'].copy()

    # Fill in the missing values for Average Heart Rate (bpm) in df_cycle with the average heart rate
    df_cycle['Average Heart Rate (bpm)'].fillna(int(avg_hr_cycle), inplace=True)

    # Count the missing values for all columns in df_cycle
    missing_values_count_cycle = df_cycle.isnull().sum()
    print("\nMissing Values Count in df_cycle:")
    print(missing_values_count_cycle)

    # Concatenate df_run with df_walk and df_cycle using append(), then sort based on the index in descending order
    df_walk = df_activities[df_activities['Type'] == 'Walking']
    df_cycle = df_activities[df_activities['Type'] == 'Cycling']
    df_run = df_activities[df_activities['Type'] == 'Running']

    df_run_walk_cycle = df_run.append([df_walk, df_cycle]).sort_index(ascending=False)

    # Group by activity type and sum the relevant columns
    dist_climb_cols = ['Distance (km)', 'Climb (m)']
    df_totals = df_run_walk_cycle.groupby('Type')[dist_climb_cols].sum()

    # Use the stack() method to show a compact reshaped form of the full summary report
    df_summary = df_totals.stack()

    print("\nConcatenated and Summarized DataFrame:")
    print(df_summary)

    # Calculate the number of shoes Forrest would need
    your_total_km = 5224
    your_shoes = 7
    forrest_total_km = 24700
    shoes_per_km = your_shoes / your_total_km
    forrest_shoes = forrest_total_km * shoes_per_km

    print(f"Forrest Gump would need approximately {forrest_shoes:.0f} pairs of shoes for his run of 24,700 km.")
    ```

## Fun Facts

- Average distance: 11.38 km
- Longest distance: 38.32 km
- Highest climb: 982 m
- Total climb: 57,278 m
- Total number of km run: 5,224 km
- Total runs: 459
- Number of running shoes gone through: 7 pairs

## Forrest Gump's Run

- Average distance: 21.13 km
- Total number of km run: 24,700 km
- Total runs: 1169
- Number of running shoes gone through: (calculated above)
