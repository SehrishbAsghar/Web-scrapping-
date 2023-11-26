# Web-scrapping
import requests
from bs4 import BeautifulSoup
import csv

def scrape_real_estate_data(location):
    url https://www.realtor.com/rentals/'

    try:
        response = requests.get(url)
        response.raise_for_status()

        soup = BeautifulSoup(response.text, 'html.parser')

        property_data = []
        for listing in soup.find_all('div', class_='property-listing'):
            title = listing.find('h2').text.strip()
            price = listing.find('span', class_='price').text.strip()
            property_url = listing.find('a')['href'].strip()

            property_data.append({
                'Title': title,
                'Price': price,
                'URL': property_url
            })

        return property_data

    except requests.exceptions.RequestException as e:
        print(f"Error: {e}")
        return None

def save_to_csv(data, filename='real_estate_data.csv'):
    with open(filename, 'w', newline='') as csvfile:
        fieldnames = ['Title', 'Price', 'URL']
        writer = csv.DictWriter(csvfile, fieldnames=fieldnames)

        writer.writeheader()
        for property_info in data:
            writer.writerow(property_info)

if __name__ == '__main__':
    location = 'your-designated-location'
    real_estate_data = scrape_real_estate_data(location)

    if real_estate_data:
        save_to_csv(real_estate_data)
        print('Scraping successful. Data saved to real_estate_data.csv')
