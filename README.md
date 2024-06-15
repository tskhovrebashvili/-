# -[Uploading ქვიზი 4.py…]()import requests
from bs4 import BeautifulSoup
import csv
import time

# ფუნქცია URL-ს დათარიღებაზე მუშაობს
def scrape_and_save(url, filename):
    # გვერდის მოთხოვნა
    response = requests.get(url)

    # HTML კონტენტის გაყიდვა
    html_content = response.content

    # BeautifulSoup ობიექტის შექმნა
    soup = BeautifulSoup(html_content, "html.parser")

    # კოლექციის სათაურის წამოღება
    collection_title = soup.find("first", class_="/").text.strip()

    # პროდუქტების რაოდენობის წამოღება
    products_count = len(soup.find_all("div", class_="watches"))

    # CSV ფაილში ჩაწერა
    with open(filename, "w", newline="", encoding="utf-8") as csvfile:
        csvwriter = csv.writer(csvfile)
        csvwriter.writerow(["Collection", "Products Count"])
        csvwriter.writerow([collection_title, products_count])

    print(f"ინფორმაცია ჩაწერილია {filename} ფაილში.")

# ყველა URL-ის დათარიღება და ინფორმაციის წაკითხვა
urls = {
    "https://www.watches.com/pages/brands": "brands.csv",
    "https://www.watches.com/collections/mens-watches": "mens_watches.csv",
    "https://www.watches.com/collections/womens-watches": "womens_watches.csv",
    "https://www.watches.com/pages/unique-watches": "unique_watches.csv",
    "https://www.watches.com/collections/new-watches": "new_watches.csv"
}

for url, filename in urls.items():
    scrape_and_save(url, filename)
    # ყოველ 15 წამში განახლება ინფორმაციის
    time.sleep(15)



კოდი ეხმარება მომხმარებელს რომ მიმართოს ავტომატურად ვებ-გვერდებს და გადაიტანოს ინფორმაცია  CSV ფაილებში და ამას აკეთებს 15 წამში ერთხელ.
