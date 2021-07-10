# Bikeshare_mohamed_elrodainy
This is my beginner's project with python on bikeshare systems for three cities in USA (Chicago, New York City, Washington)
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
    while True:
        try:
            city = input('Which city you would like to explore? ... choose from this list and include spaces when writing ["Chicago", "New York City", "Washington"]: ').lower()
            # handling the city input values
            while city not in ['chicago', 'new york city', 'washington']:
                
                print('The city you entered is not here, try one from the list')
                
                city = input('Which city you would like to explore? ... choose from this list and include spaces when writing ["Chicago", "New York City", "Washington"]: ').lower()
    # TO DO: get user input for month (all, january, february, ... , june)
            month = input('Which month? January, february, march ... : ').lower()
            # handling the month input values
            while month not in ['january', 'february', 'march', 'april', 'may', 'june', 'july', 'august', 'september', 'october', 'november', 'december']:
                
                print('That is not a valid month name')
                
                month = input('Which month? January, february, march ... : ').lower()
            
    # TO DO: get user input for day of week (all, monday, tuesday, ... sunday)     
            day = int(input('Which weekday? refer to week days with integers eg.. sunday = 1 : ' ))
            
            # handling the day input values
            while day not in np.arange(1, 8):
                print('There are only 7 days in a week, enter a number between 1 and 7')
                day = int(input('Which weekday? refer to week days with integers eg.. sunday = 1 : ' ))
            break                
                
                
        except ValueError:
            print('That is an invalid value for this input')
            
        except ZeroDivisionError:
            print('This input can\'t take zero as a value')
            
        except KeyboardInterrupt:
            print('You are welcome anytime!')
            break
            
            
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
    # Loading the data from CSV files
    df = pd.read_csv(CITY_DATA[city])
    
    # Converting the start time column into datetime form using .to_datetime method
    df['Start Time'] = pd.to_datetime(df['Start Time'])
    
    # Extracting month and weekday from start time column
    df['month'] = df['Start Time'].dt.month
    df['weekday'] = df['Start Time'].dt.weekday_name
    
    # filter by month if chosen
    if month != 'all':
        # use of the month's index to get the corresponding integer
        months = ['january', 'february', 'march', 'april', 'may', 'june', 'july', 'august', 'september', 'october', 'november', 'december']
        month = months.index(month) + 1
        
        # Create the new data frame containing the new month filter
        df[df['month'] == month]
        
    # filter by day if chosen
    if day != all:
        # create the new data frame containing the new day filter
        df[df['weekday'] == day]
        
    return df


def time_stats(df):
    """Displays statistics on the most frequent times of travel."""

    print('\nCalculating The Most Frequent Times of Travel...\n')
    start_time = time.time()

    # TO DO: display the most common month\
    
    # list of months to convert the month back to it's name
    months = ['january', 'february', 'march', 'april', 'may', 'june', 'july', 'august', 'september', 'october', 'november', 'december']
    
    # getting the most common month using .mode() method
    most_common_month = df['month'].mode()[0]
    
    # printing the most common month
    print('The most common month is: {} '.format(months[most_common_month - 1].title()))
    
    # TO DO: display the most common day of week
    # getting the most common day using the .mode() method
    most_common_day = df['weekday'].mode()[0]
    
    # printing the most common day
    print('The most common day is: {}'.format(most_common_day))
    
    # TO DO: display the most common start hour
    # extracting the hours column from the start time column
    df['hour'] = df['Start Time'].dt.hour
    
    # getting the most common hour using the .mode() method
    most_common_hour = df['hour'].mode()[0]
    
    # printing the most common hour
    print('The most common hour is: {}'.format(most_common_hour))
    
    
    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def station_stats(df):
    """Displays statistics on the most popular stations and trip."""

    print('\nCalculating The Most Popular Stations and Trip...\n')
    start_time = time.time()

    # TO DO: display most commonly used start station
    # getting the most commonly used start station using the .mode() method
    most_commonly_used_start_station = df['Start Station'].mode()[0]
    
    # printing the most commonly used start station
    print('The most commonly used start station is: {}'.format(most_commonly_used_start_station))
    
    # TO DO: display most commonly used end station
    # getting the most commonly used end station using .mode() method
    most_commonly_used_end_station = df['End Station'].mode()[0]
    
    # printing the most commonly used end station
    print('The most commonly used end station is: {}'.format(most_commonly_used_end_station))
    
    # TO DO: display most frequent combination of start station and end station trip
    # most frequent combination of start and end stations
    most_frequent_combination = df.groupby(['Start Station', 'End Station']).size().sort_values(ascending=False)
    
    # printing the most frequent combination
    print('The most frequent combination of start and end stations is: {}'.format(most_frequent_combination.index[0]))
    
    
    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def trip_duration_stats(df):
    """Displays statistics on the total and average trip duration."""

    print('\nCalculating Trip Duration...\n')
    start_time = time.time()

    # TO DO: display total travel time
    # calculating the total trip time in seconds
    total_trip_time = df['Trip Duration'].sum()
    
    # printing the total trip duration
    print('The total trip duartion in seconds is: {}'.format(total_trip_time))
    
    # TO DO: display mean travel time
    mean_travel_time = df['Trip Duration'].mean()
    print('The mean travel time in seconds is: {}'.format(mean_travel_time))

    
    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def user_stats(df):
    """Displays statistics on bikeshare users."""

    print('\nCalculating User Stats...\n')
    start_time = time.time()

    # TO DO: Display counts of user types
    # calculating the user types values count
    user_types_count = df['User Type'].value_counts()
    
    # printing the user types values count
    print('User types counts are: {}'.format(user_types_count))
    
    # TO DO: Display counts of gender
    # calculating the gender values count
    gender_counts = df['Gender'].value_counts()

    # printing the gender values count
    print('Gender counts are: {}'.format(gender_counts))

    # TO DO: Display earliest, most recent, and most common year of birth
    # calculating the ealiest birth year
    ealiest_year = df['Birth Year'].min()

    # calculating the most recent birth year
    most_recent_year = df['Birth Year'].max()

    # calculating the most common birth year
    most_common_year = df['Birth Year'].mode()[0]

    # printing the above birth year statistics
    print('The earliest year is: {}, the most recent year is: {} and the most common year is: {}'.format(ealiest_year, most_recent_year, most_common_year))
    
    
    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)

def display_data(frame):
    # purpose is to view more statistics
    # asking the user if he wants to view statistics or not
    view_data = input('Would you like to view 5 rows of individual trip data? yes or no: ').lower()
    # handling the input values
    while view_data not in ['yes', 'no']:
        print('This is not a valid answer')
        view_data = input('Would you like to view 5 rows of individual trip data? yes or no: ').lower()
    # assuming the start number of viewed rows as 5
    start_loc = 5
    # looping to view statistics
    while (view_data == 'yes'):
        print(frame.iloc[:start_loc])
        start_loc += 5
        # this will ask the user again if he wants 5 more or not
        view_data = input('Would you like to continue? yes or no: ').lower()
        

def main():
    while True:
        city, month, day = get_filters()
        df = load_data(city, month, day)

        time_stats(df)
        station_stats(df)
        trip_duration_stats(df)
        # this if statement will stop the statistics in case washington is picked
        if city != 'washington':
            user_stats(df)
        else:
            print('Sorry, we do not have gender or birth year data for washington city so, unfortunately our statistics will stop here')
        display_data(df)

        restart = input('\nWould you like to restart? Enter yes or no.\n')
        if restart.lower() != 'yes':
            break


if __name__ == "__main__":
	main()
    

