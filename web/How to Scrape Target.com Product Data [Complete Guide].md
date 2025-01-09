
# How to Scrape Target.com Product Data [Complete Guide]

Scraping product data from Target.com can unlock valuable insights for market analysis, price monitoring, and competitor tracking. This guide will teach you how to build a scalable web scraper using **Puppeteer**, **Cheerio**, and **ExcelJS**, along with **ScraperAPI** for proxy management to avoid getting blocked.

---

### Stop Wasting Time on Proxies and CAPTCHAs!

ScraperAPI’s simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more.  
👉 [Start your free trial today!](https://bit.ly/Scraperapi)

---

## What You’ll Learn

By the end of this tutorial, you’ll be able to scrape Target.com for product data and export it to an Excel file. Specifically, we’ll extract the following fields:

- Product Title
- Brand
- Current Price
- Regular Price
- Average Rating
- Total Reviews
- Product Link

Let’s get started!

---

## Prerequisites

Ensure you have the following set up before proceeding:

1. **[Node.js](https://nodejs.org/en/download)** (v18+ recommended)
2. Basic knowledge of JavaScript and Node.js
3. A **ScraperAPI account** – [Sign up](https://bit.ly/Scraperapi) and get 5,000 free API credits to start!

---

## Step 1: Set Up the Project

1. Create a project folder and initialize it as a Node.js project:

   ```bash
   mkdir target-dot-com-scraper
   cd target-dot-com-scraper
   npm init -y
   ```

   This will create a `package.json` file in the folder.

2. Create a file named `index.js` for your scraper code.

---

## Step 2: Install Dependencies

Install the necessary libraries for building the scraper:

- **Puppeteer**: To control a headless browser.
- **Cheerio**: For HTML parsing and DOM traversal.
- **ExcelJS**: To export data to an Excel file.

Run the following command to install them:

```bash
npm install puppeteer cheerio exceljs
```

---

## Step 3: Identify DOM Selectors

1. Go to [Target.com](https://www.target.com), search for “headphones,” and inspect the HTML structure (Right-click → Inspect or press `F12`).

2. Identify the CSS selectors for the required fields:
   - Product Title: `a.csOImU`
   - Brand: `a.cnZxgy`
   - Current Price: `span[data-test='current-price'] span`
   - Regular Price: `span[data-test='comparison-price'] span`
   - Reviews: `.hMtWwx`

---

## Step 4: Write the Web Scraper

### Configure ScraperAPI Proxy

ScraperAPI helps bypass IP bans and CAPTCHAs by rotating proxies. Add your ScraperAPI credentials:

```javascript
const PROXY_USERNAME = 'scraperapi';
const PROXY_PASSWORD = '<API_KEY>'; // Replace with your API key
const PROXY_SERVER = 'proxy-server.scraperapi.com';
const PROXY_PORT = '8001';
```

### Puppeteer Script

The Puppeteer script will handle opening the Target.com search results page, scrolling to load all products, and downloading the HTML content:

```javascript
const puppeteer = require("puppeteer");

const TARGET_URL = 'https://www.target.com/s?searchTerm=headphones';

const webScraper = async () => {
  const browser = await puppeteer.launch({
    args: [`--proxy-server=http://${PROXY_SERVER}:${PROXY_PORT}`],
  });

  const page = await browser.newPage();

  // Authenticate with ScraperAPI proxy
  await page.authenticate({
    username: PROXY_USERNAME,
    password: PROXY_PASSWORD,
  });

  // Set User-Agent to mimic a real browser
  await page.setExtraHTTPHeaders({
    "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 13.5; rv:109.0) Gecko/20100101 Firefox/117.0",
  });

  // Load Target search results
  await page.goto(TARGET_URL, { timeout: 60000 });

  // Scroll to load all products
  await page.evaluate(async () => {
    await new Promise((resolve) => {
      let totalHeight = 0;
      const distance = 100;

      const timer = setInterval(() => {
        const scrollHeight = document.body.scrollHeight;
        window.scrollBy(0, distance);
        totalHeight += distance;

        if (totalHeight >= scrollHeight) {
          clearInterval(timer);
          resolve();
        }
      }, 100);
    });
  });

  const html = await page.content();
  await browser.close();

  return html;
};
```

---

## Step 5: Extract Product Data

Use Cheerio to parse the HTML and extract product data:

```javascript
const cheerio = require("cheerio");

const extractProductData = (html) => {
  const $ = cheerio.load(html);
  const products = [];

  $(".dOpyUp").each((_, el) => {
    const title = $(el).find("a.csOImU").text();
    const brand = $(el).find("a.cnZxgy").text();
    const currentPrice = $(el).find("span[data-test='current-price'] span").text();
    const regularPrice = $(el).find("span[data-test='comparison-price'] span").text();
    const reviewsText = $(el).find(".hMtWwx").text();

    const ratingRegex = /(\d+(\.\d+)?)/;
    const reviewCountRegex = /(\d+) ratings/;
    const rating = reviewsText.match(ratingRegex)?.[0] || null;
    const reviewCount = reviewsText.match(reviewCountRegex)?.[1] || null;

    products.push({
      title,
      brand,
      currentPrice,
      regularPrice,
      averageRating: parseFloat(rating),
      totalReviews: parseInt(reviewCount),
    });
  });

  return products;
};
```

---

## Step 6: Export Data to Excel

Save the extracted product data in an Excel file using ExcelJS:

```javascript
const Excel = require("exceljs");
const path = require("path");

const exportToExcel = async (products) => {
  const workbook = new Excel.Workbook();
  const worksheet = workbook.addWorksheet("Headphones");

  worksheet.columns = [
    { header: "Title", key: "title" },
    { header: "Brand", key: "brand" },
    { header: "Current Price", key: "currentPrice" },
    { header: "Regular Price", key: "regularPrice" },
    { header: "Average Rating", key: "averageRating" },
    { header: "Total Reviews", key: "totalReviews" },
  ];

  products.forEach((product) => {
    worksheet.addRow(product);
  });

  const filePath = path.resolve(__dirname, "products.xlsx");
  await workbook.xlsx.writeFile(filePath);

  console.log("Data exported to products.xlsx");
};
```

---

## Step 7: Integrate Everything

Combine all the pieces into a single script:

```javascript
const main = async () => {
  try {
    const html = await webScraper();
    const products = extractProductData(html);
    await exportToExcel(products);

    console.log(`${products.length} products exported successfully!`);
  } catch (error) {
    console.error("Error:", error);
  }
};

main();
```

Run the scraper with:

```bash
node index.js
```

---

## Conclusion

This guide showed how to scrape product data from Target.com using Puppeteer, Cheerio, and ScraperAPI. By combining automated web scraping tools and scalable proxy management, you can build efficient scrapers for various e-commerce platforms.

👉 **Start your free trial with ScraperAPI today!**  
[https://bit.ly/Scraperapi](https://bit.ly/Scraperapi)
