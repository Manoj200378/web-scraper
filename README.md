import requests
from bs4 import BeautifulSoup
import json

url = input("Enter the URL of the website you want to scrape: ")

try:
    response = requests.get(url)
    response.raise_for_status()  # this will raise an error for 404/500/etc
    html = response.text
except Exception as e:
    print("Couldn’t fetch the page:", e)
    exit()

# Only happens if the request succeeded
soup = BeautifulSoup(html, 'html.parser')
all_links = soup.find_all('a')

collected_data = []

for tag in all_links:
    text = tag.text.strip()
    link = tag.get('href')
    if text != '' and link is not None:
        collected_data.append({
            "text": text,
            "link": link
        })

with open("scraped_links.json", "w", encoding="utf-8") as file:
    json.dump(collected_data, file, indent=4)

print("Scraping done! Saved", len(collected_data), "links to scraped_links.json")

