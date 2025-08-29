import requests
import time

# A list of cryptocurrency IDs from the CoinGecko API
# You can find more IDs on their website: https://www.coingecko.com/en/api
CRYPTO_IDS = ['bitcoin', 'ethereum', 'solana']

def get_crypto_prices():
    """Fetches crypto prices from the CoinGecko API."""
    
    # Format the list of IDs into a comma-separated string for the API URL
    ids_string = ",".join(CRYPTO_IDS)
    
    # The API endpoint URL
    url = f"https://api.coingecko.com/api/v3/simple/price?ids={ids_string}&vs_currencies=usd,inr"
    
    try:
        print("Fetching latest prices...")
        response = requests.get(url)
        response.raise_for_status()  # Raise an exception for bad status codes (4xx or 5xx)
        
        # Convert the JSON response to a Python dictionary
        prices = response.json()
        
        print("--- Latest Cryptocurrency Prices ---")
        for coin_id, price_data in prices.items():
            # Format the prices with commas for readability
            price_usd = f"${price_data['usd']:,}"
            price_inr = f"₹{price_data['inr']:,}"
            
            # Print the formatted price information
            print(f"{coin_id.capitalize():<10}: {price_usd:<15} | {price_inr}")
            
    except requests.exceptions.RequestException as e:
        print(f"Error: Could not fetch data from the API. Please check your connection. Details: {e}")
    except KeyError:
        print("Error: Received an unexpected response format from the API.")

if __name__ == "__main__":
    get_crypto_prices()