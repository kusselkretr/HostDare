
# How to Scrape eBay Using Python for Product Data Extraction

Web scraping eBay can unlock valuable data about sellers, prices, ratings, and more. Whether you're analyzing market trends or monitoring competitors, collecting and analyzing this data can give you an edge in your industry. Over time, you can identify patterns in product popularity, seller behavior, and buyer preferences.

In this guide, we'll build an **eBay scraper** using Python to extract product information seamlessly. This step-by-step approach will teach you how to gather data on target products effectively.

---

### Stop Wasting Time on Proxies and CAPTCHAs!

ScraperAPI’s simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more.  
👉 [Start your free trial today!](https://bit.ly/Scraperapi)

---

## Prerequisites

Ensure you have **Python 3.x** installed on your system. If not, download it from [Python's official website](https://www.python.org/downloads/).

We'll also need the following Python libraries:
- **`requests`**: For establishing HTTP connections and retrieving raw HTML data.
- **`BeautifulSoup`**: For parsing and extracting the desired information from the HTML.

### Installation
To set up your environment:
1. Create a project folder.
2. Install the required libraries:
   ```bash
   pip install beautifulsoup4
   pip install requests
   ```

### Initial Setup

Create a Python file (e.g., `ebay_scraper.py`) in your project folder. Let's test if we can scrape eBay without being blocked.

```python
import requests
from bs4 import BeautifulSoup

target_url = "https://www.ebay.com/itm/182905307044"  # Example product page

resp = requests.get(target_url)

print(resp.status_code)
```

If the script runs successfully, the output should display a **200 status code**, indicating that the connection was successful.

---

## Identify Target Data

Before coding, identify the data elements you want to scrape. Here are the seven key elements we'll extract:
1. **Product Images**
2. **Product Title**
3. **Rating**
4. **Price**
5. **Listed Price**
6. **Discount or Savings**
7. **Shipping Charges**

Using browser developer tools (right-click → Inspect), analyze the HTML structure of the target page to locate these elements.

---

## Step-by-Step Data Extraction

### Extracting Product Title

The title is stored inside an `<h1>` tag with the class `x-item-title__mainTitle`. Here’s how to extract it:

```python
soup = BeautifulSoup(resp.text, 'html.parser')
title = soup.find("h1", {"class": "x-item-title__mainTitle"}).text
print("Product Title:", title)
```

---

### Extracting Rating

Ratings are stored in a `<span>` tag with the class `ebay-review-start-rating`:

```python
rating = soup.find("span", {"class": "ebay-review-start-rating"}).text.strip()
print("Rating:", rating)
```

---

### Extracting Actual Price

The price is stored in a `<span>` tag with the attribute `itemprop="price"`. Use the following code to extract it:

```python
price = soup.find("span", {"itemprop": "price"}).text
print("Price:", price)
```

---

### Extracting Listed Price and Discount

Both the listed price and discount are located in a `<div>` tag with the class `x-additional-info`:

```python
box = soup.find("div", {"class": "x-additional-info"})
list_price = box.find("span", {"class": "ux-textspans--STRIKETHROUGH"}).text
discount = box.find("span", {"class": "ux-textspans--EMPHASIS"}).text
print("Listed Price:", list_price)
print("Discount:", discount)
```

---

### Extracting Shipping Charges

Shipping charges are stored in a `<span>` tag with the class `ux-textspans--BOLD`, nested inside a `<div>` tag with the ID `SRPSection`:

```python
shipping_price = soup.find("div", {"id": "SRPSection"}).find("span", {"class": "ux-textspans--BOLD"}).text
print("Shipping Price:", shipping_price)
```

---

### Extracting Product Images

All product images are stored in `<div>` tags with the class `ux-image-carousel-item`. Each `<img>` tag contains the image URL:

```python
images = soup.find_all("div", {"class": "ux-image-carousel-item"})
image_urls = [image.find("img").get("data-src") for image in images]
print("Images:", image_urls)
```

---

## Handling Large-Scale Scraping

Scraping eBay at scale requires more advanced methods since eBay may block repeated requests from the same IP. To bypass these restrictions, use **ScraperAPI** to rotate IPs and avoid detection.

---

### Use ScraperAPI for Seamless Scraping

With ScraperAPI, you don’t have to worry about retries, headers, or proxy management. Here’s how to integrate it:

1. Sign up for **ScraperAPI** and get your free API key: 👉 [Sign up here](https://bit.ly/Scraperapi).

2. Replace the target URL in your script with ScraperAPI’s URL format:
   ```python
   target_url = "https://api.scraperapi.com?api_key=SCRAPE9837861&url=https://www.ebay.com/itm/182905307044&render=false"
   ```

3. Update your script to use ScraperAPI:

   ```python
   import requests
   from bs4 import BeautifulSoup

   l = []
   o = {}
   image_list = []

   target_url = "https://api.scraperapi.com?api_key=SCRAPE9837861&url=https://www.ebay.com/itm/182905307044&render=false"
   resp = requests.get(target_url)
   soup = BeautifulSoup(resp.text, 'html.parser')

   o["title"] = soup.find("h1", {"class": "x-item-title__mainTitle"}).text
   o["rating"] = soup.find("span", {"class": "ebay-review-start-rating"}).text.strip()
   o["actual_price"] = soup.find("span", {"itemprop": "price"}).text
   box = soup.find("div", {"class": "x-additional-info"})
   o["list_price"] = box.find("span", {"class": "ux-textspans--STRIKETHROUGH"}).text
   o["discount"] = box.find("span", {"class": "ux-textspans--EMPHASIS"}).text
   o["shipping_price"] = soup.find("div", {"id": "SRPSection"}).find("span", {"class": "ux-textspans--BOLD"}).text
   images = soup.find_all("div", {"class": "ux-image-carousel-item"})
   image_urls = [image.find("img").get("data-src") for image in images]
   o["images"] = image_urls
   l.append(o)

   print(l)
   ```

With **ScraperAPI**, you’ll achieve over a 99% success rate while scraping eBay. It’s efficient, scalable, and perfect for large-scale projects.

---

## Conclusion

In this tutorial, we learned how to scrape various data points from eBay using Python and **BeautifulSoup**. While scraping a single page is straightforward, scaling the process requires additional tools like **ScraperAPI** to handle IP rotation and CAPTCHA challenges.

For a seamless and reliable scraping experience, integrate **ScraperAPI** into your workflow and start extracting data efficiently.  
👉 [Start your free trial today!](https://bit.ly/Scraperapi)
