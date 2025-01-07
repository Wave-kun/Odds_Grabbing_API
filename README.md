# This is an API for grabbing the odds of Premier League from bookmakers and showing in a json format
# Odds_Grabbing_API

import requests
import json

# Your API key from The Odds API
API_KEY = "9ae3ba55cc6169dadef1e3a5d1b4dc37"
# URL endpoint for fetching soccer odds (example for soccer)
API_URL = 'https://api.the-odds-api.com/v4/sports/soccer_epl/odds'

def fetch_odds_data(api_key):
    params = {
        'apiKey': api_key,
        'regions': 'eu',  # Europe region for soccer leagues
        'markets': 'h2h', # head-to-head odds
        'oddsFormat': 'decimal'
    }
    
    try:
        response = requests.get(API_URL, params=params)
        response.raise_for_status()  # Check if the request was successful
        odds_data = response.json()
        
        # Print out the fetched data
        for game in odds_data:
            print("Match:", game['home_team'], "vs", game['away_team'])
            for bookmaker in game['bookmakers']:
                print("Bookmaker:", bookmaker['title'])
                for market in bookmaker['markets']:
                    print("Odds:")
                    for outcome in market['outcomes']:
                        print(f"{outcome['name']}: {outcome['price']}")
            print("-" * 30)  # Divider for readability
            
        return odds_data
    
    except requests.exceptions.RequestException as e:
        print("Error fetching data:", e)
        return None

# Run the function
odds_data = fetch_odds_data(API_KEY)

# Optional: Save the data to a JSON file for later analysis
with open('football_odds_data.json', 'w') as f:
    json.dump(odds_data, f, indent=4)
