```python
# Using the Google Custom Search API scrape 100 images of each of the selected product categories.
# Save the images in a folder named as the product category.
# The product categories are Vanities, Bathtubs, Showers, Toilets, Bath Faucets, Sink Faucets, Vanity Lighting, Bathroom Shelves, Shower Walls, Bath Fans.
# The images should be saved in a folder named as the product category.

import requests
import os

from dotenv import load_dotenv

# Create a folder for each product category
def create_folders(categories):
    for category in categories:
        try:
            if not os.path.exists(category):
                os.makedirs(category)
        except OSError:
            print("Error: Creating directory. " + category)


# Get the images from the Google Custom Search API
def get_images(categories):
    API_KEY = os.getenv("GOOGLE_API_KEY")
    SEARCH_ENGINE_ID = os.getenv("SEARCH_ENGINE_ID")
    main_url = f"https://www.googleapis.com/customsearch/v1?key={API_KEY}&cx={SEARCH_ENGINE_ID}&searchType=image&imgSize=large&num=10&q="

    for category in categories:
        print("Category:", category)
        category_url = f"{main_url}{category}"
        response = requests.get(category_url)
        if response.status_code == 200:
            data = response.json()
            items = data["items"]
            urls = []
            for item in items:
                urls.append(item["link"])

            for url in urls:
                print("URL:", url)
                response = requests.get(url)
                filename = url.split("/")[-1]
                with open(f"{category}/{filename}", "wb") as f:
                    f.write(response.content)
        else:
            print(response.text)


def main():
    # Load environment variables for api key
    load_dotenv()

    # Create a list of product categories
    """        
        "Bathroom Vanities",
        "Bath Faucets",
        "Bathtubs",
        "Showers",
        "Toilets",
        "Bathroom Storage",
        "Bathroom Sinks",
        "Bath Accessories",
        "Bathroom Mirrors",
        "Bathroom Exhaust Fans",     
    """
    product_categories: list[str] = [
        "Bathroom Vanities",
        "Bath Faucets",
        "Bathtubs",
        "Showers",
        "Toilets",
        "Bathroom Storage",
        "Bathroom Sinks",
        "Bath Accessories",
        "Bathroom Mirrors",
        "Bathroom Exhaust Fans",
    ]

    # Create a folder for each product category if it does not exist
    create_folders(product_categories)

    # Get the images for each product category
    get_images(product_categories)


if __name__ == "__main__":
    main()

```