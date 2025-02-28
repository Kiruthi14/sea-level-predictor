import pandas as pd
import matplotlib.pyplot as plt
from scipy.stats import linregress

def read_data():
    # Read the data from the CSV file
    df = pd.read_csv('epa-sea-level.csv')
    return df

def create_scatter_plot(df):
    # Create a scatter plot of Year vs CSIRO Adjusted Sea Level
    plt.scatter(df['Year'], df['CSIRO Adjusted Sea Level'])
    plt.xlabel('Year')
    plt.ylabel('Sea Level (inches)')
    plt.title('Rise in Sea Level')

def plot_line_of_best_fit(df):
    # Calculate the line of best fit using all data
    slope, intercept, r_value, p_value, std_err = linregress(df['Year'], df['CSIRO Adjusted Sea Level'])
    
    # Plot the line of best fit for all data
    years_extended = range(1880, 2051)  # Predict through year 2050
    sea_levels_extended = [slope * year + intercept for year in years_extended]
    plt.plot(years_extended, sea_levels_extended, label='Best Fit Line (1880-2050)', color='blue')

def plot_line_of_best_fit_since_2000(df):
    # Filter data for years from 2000 onwards
    df_recent = df[df['Year'] >= 2000]
    
    # Calculate the line of best fit for data from 2000 onward
    slope, intercept, r_value, p_value, std_err = linregress(df_recent['Year'], df_recent['CSIRO Adjusted Sea Level'])
    
    # Plot the line of best fit for data from 2000 onwards
    sea_levels_recent = [slope * year + intercept for year in range(2000, 2051)]
    plt.plot(range(2000, 2051), sea_levels_recent, label='Best Fit Line (2000-2050)', color='red')

def save_plot():
    # Save the plot as a PNG file
    plt.legend()
    plt.tight_layout()
    plt.savefig('sea_level_predictor.png')
    plt.show()

def main():
    # Step 1: Read the data
    df = read_data()
    
    # Step 2: Create the scatter plot
    create_scatter_plot(df)
    
    # Step 3: Plot the line of best fit (using all data)
    plot_line_of_best_fit(df)
    
    # Step 4: Plot the line of best fit (using data from 2000 onward)
    plot_line_of_best_fit_since_2000(df)
    
    # Step 5: Save and show the plot
    save_plot()

if __name__ == "__main__":
    main()
