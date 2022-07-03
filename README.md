# Investigate-Data-Set

In this project, I made use of Python to explore data related to bike share systems for three major cities in the United States—Chicago, New York City, and Washington. 

I wrote codes to import the data and answered interesting questions about it by computing descriptive statistics.

I also wrote a script that takes in raw input to create an interactive experience in the terminal to present these statistics.

Below is the code for this project.


import time
import pandas as pd
import numpy as np

CITY_DATA = { 'chicago': 'chicago.csv',
              'new york city': 'new_york_city.csv',
              'washington': 'washington.csv' }

def get_filters():
    """
    Asks user to specify a city, month, and day to analyze.

    Returns:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    """
    print('Hello! Let\'s explore some US bikeshare data!')
    # TO DO: get user input for city (chicago, new york city, washington). HINT: Use a while loop to handle invalid inputs
    
    CITIES = ['chicago', 'new york city', 'washington']
    while True:
      city = input('What city data do you want to explore? Chicago, New York City, Washington?\n').lower()
      if city in CITIES:
            print('You have selected {} data'.format(city))
            print('Lets\'s go!')
            break
      else:
        print('You have picked a city outside the scope.')
        print('The options are: \n1. Washington \n2. Chicago \n3. New york city. \nNow let\'s try again.')
               
                
                                       
                                       
    # TO DO: get user input for month (all, january, february, ... , june)
    MONTHS = ['january', 'february', 'march', 'april', 'may', 'june', 'all']
    while True:
        month = input('Select one month from january to june. Type \"all\" if you want to see for all the months:\n').lower()
        if month in MONTHS:
            print('You have selected {} month data'.format(month))
            print('Lets\'s go!')
            break
        else:
            print('You have made a wrong selection.')
            print('The options are from january to june. \nNow let\'s try again.')
                    

    # TO DO: get user input for day of week (all, monday, tuesday, ... sunday)
        
        
    DAYS = ['sunday', 'monday', 'tuesday', 'wednesday', 'thursday', 'friday', 'saturday', 'all']
    
    while True:
          day = input('What day of the week data do you want to see? Type \"all\" if you want to see all the days:\n').lower()
          if day in DAYS:
            print('You have selected {} day data'.format(day))
            print('Lets\'s go!')
            break
          else:
            print('You have made a wrong selection.')
            print('The options are from sunday to saturday. \nNow let\'s try again.')
                                       
    print('-'*40)    
    return city, month, day




def load_data(city, month, day):
    """
    Loads data for the specified city and filters by month and day if applicable.

    Args:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    Returns:
        df - Pandas DataFrame containing city data filtered by month and day
    """
    df = pd.read_csv(CITY_DATA[city])
    
    
    df['Start Time'] = pd.to_datetime(df['Start Time'])
    df['month'] = df['Start Time'].dt.month_name()
    df['day_of_week'] = df['Start Time'].dt.weekday_name
    df['Start Hour']= df['Start Time'].dt.hour 


    return df


def time_stats(df):
    """Displays statistics on the most frequent times of travel."""

    print('\nCalculating The Most Frequent Times of Travel...\n')
    start_time = time.time()
    
    
    
    # TO DO: display the most common month
    mode_month = df['month'].mode()[0]
    print('The most common month is {}\n'.format(mode_month))             
                  

    # TO DO: display the most common day of week
    mode_week = df['day_of_week'].mode()[0]
    print('The most common day of the week is {}\n'.format(mode_week))                  

    # TO DO: display the most common start hour              
    mode_hour = df['Start Hour'].mode()[0]
    print('The most common start hour is {}\n'.format(mode_hour))                   


    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def station_stats(df):
    """Displays statistics on the most popular stations and trip."""

    print('\nCalculating The Most Popular Stations and Trip...\n')
    start_time = time.time()
    
    # TO DO: display most commonly used start station
    mode_start_station = df['Start Station'].mode()[0]
    print('The most commonly used start station is {}\n'.format(mode_start_station))


    # TO DO: display most commonly used end station
    mode_end_station = df['End Station'].mode()[0]
    print('The most commonly used end station is {}\n'.format(mode_end_station))

    # TO DO: display most frequent combination of start station and end station trip
    mode_comb_station = df[['Start Station', 'End Station']].mode()
    print('The most frequent combination of start and end station trip is {}\n'.format(mode_comb_station))

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def trip_duration_stats(df):
    """Displays statistics on the total and average trip duration."""

    print('\nCalculating Trip Duration...\n')
    start_time = time.time()

    # TO DO: display total travel time
    total_travel_time = df['Trip Duration'].sum()
    print('The total travel time is {}\n'.format(total_travel_time))

    # TO DO: display mean travel time
    mean_travel_time = df['Trip Duration'].mean()
    print('The mean travel time is {}\n'.format(mean_travel_time))
    

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def user_stats(df):
    """Displays statistics on bikeshare users."""

    print('\nCalculating User Stats...\n')
    start_time = time.time()

    # TO DO: Display counts of user types
    user_type_count = df['User Type'].value_counts()
    print('There are {} user types count\n'.format(user_type_count))

    # TO DO: Display counts of gender 
    if 'Gender' not in df:
        print('There are no gender information for Washington')
        print('Hence, there are 0 gender counts\n')
    else:
        gender_count = df['Gender'].value_counts()
        print('There are {} gender counts\n'.format(gender_count))

    # TO DO: Display earliest, most recent, and most common year of birth
    if 'Birth Year' not in df:
        print('There are no birth year information for washington')
        print('Hence, there is no most recent birth year\n')
    else:
        max_birth_year = df['Birth Year'].max()
        print('The most recent birth year is {}\n'.format(max_birth_year))
              
    if 'Birth Year' not in df:
        print('There is no earliest birth year.')
    else:
        min_birth_year = df['Birth Year'].min()
        print('The earliest birth year is {}\n'.format(min_birth_year))      
            
    if 'Birth Year' not in df:
        print('There is no most common birth year.\n')
    else:
        mode_birth_year = df['Birth Year'].mode()[0]
        print('The most common birth year is {}\n'.format(mode_birth_year))
    
    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)

    
    number = 5
    raw_data = input('Would you like to see 5 lines of the raw data? yes or no?\n').lower()
    while True:
        if raw_data == 'no':
            break
        elif raw_data == 'yes':
            print(df.head(number))      
            next_data = input('Would you like to see the next 5 lines of the raw data? yes or no?\n').lower() 
            if next_data == 'yes':
                number += 5
                raw_data = next_data    
            elif next_data == 'no':
                break
            else:
                print('Please enter a valid input. Type \'yes\' or \'no\'')
                continue 
        else:
            print('Please enter a valid input. Type \'yes\' or \'no\'')
            raw_data = input('Would you like to see 5 lines of the raw data? yes or no?\n').lower()

        
    
def main():
    while True:
        city, month, day = get_filters()
        df = load_data(city, month, day)

        time_stats(df)
        station_stats(df)
        trip_duration_stats(df)
        user_stats(df)

        restart = input('\nWould you like to restart? Enter yes or no.\n')
        if restart.lower() != 'yes':
            break


if __name__ == "__main__":
	main()
