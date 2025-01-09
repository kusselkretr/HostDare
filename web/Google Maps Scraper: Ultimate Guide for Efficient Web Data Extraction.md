
# Google Maps Scraper: Ultimate Guide for Efficient Web Data Extraction

Google Maps Scraper is a versatile tool designed for efficient data scraping from Google Maps. Whether you’re looking to analyze local businesses, extract reviews, or gather geospatial data, this guide will walk you through its features, setup, and best practices for web scraping.

---

## Why Use Google Maps Scraper?

Google Maps Scraper enables you to:
- Extract valuable business details (e.g., contact info, reviews, locations).
- Automate large-scale data collection from Google Maps.
- Customize data exports to CSV, JSON, or PostgreSQL for analysis.

Effortlessly manage web scraping tasks using the command line, REST API, or a web-based interface.

---

### Stop Wasting Time on Proxies and CAPTCHAs!

ScraperAPI’s simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Google Maps, Amazon, Walmart, and more.  
👉 [Start your free trial today!](https://bit.ly/Scraperapi)

---

## Features

Google Maps Scraper comes loaded with powerful features, including:
- **Comprehensive Data Points**: Extracts business names, contact info, reviews, and more.
- **Data Export**: Save results as CSV, JSON, or directly to a PostgreSQL database.
- **Concurrent Scraping**: Handles up to 120 URLs per minute with multi-threaded execution.
- **Proxy Support**: Full support for SOCKS5/HTTP/HTTPS proxies.
- **RESTful API**: Programmatically manage scraping tasks with a detailed API.
- **Email Extraction**: Optionally scrape email addresses from business websites.
- **Fast Mode (Beta)**: Quickly extract basic data with a focus on speed.
- **Scalability**: Run on multiple machines or in the cloud using Kubernetes or AWS Lambda.

---

## Quickstart

### Using Docker (Recommended)

Google Maps Scraper is dockerized for easy setup across multiple platforms.

Run the scraper using Docker:
```bash
touch results.csv && docker run -v $PWD/example-queries.txt:/example-queries -v $PWD/results.csv:/results.csv gosom/google-maps-scraper -depth 1 -input /example-queries -results /results.csv -exit-on-inactivity 3m
```

This will generate a `results.csv` file containing parsed data.

**Optional**: To include email extraction, use the `-email` parameter:
```bash
docker run -v $PWD/example-queries.txt:/example-queries -v $PWD/results.csv:/results.csv gosom/google-maps-scraper -depth 1 -email -input /example-queries -results /results.csv -exit-on-inactivity 3m
```

---

### Using Command Line

Install and run the scraper directly on your host system (tested on Ubuntu 22.04):
```bash
git clone https://github.com/gosom/google-maps-scraper.git
cd google-maps-scraper
go mod download
go build
./google-maps-scraper -input example-queries.txt -results results.csv -exit-on-inactivity 3m
```

Results will be saved to the `results.csv` file.

---

## REST API

Google Maps Scraper provides a RESTful API for managing scraping tasks programmatically.

### Key Endpoints:
- **POST `/api/v1/jobs`**: Create a new scraping job.
- **GET `/api/v1/jobs`**: List all jobs.
- **GET `/api/v1/jobs/{id}`**: Get details of a specific job.
- **DELETE `/api/v1/jobs/{id}`**: Delete a job.
- **GET `/api/v1/jobs/{id}/download`**: Download job results as CSV.

Refer to the [API Documentation](https://localhost:8080/api/docs) for full details.

---

## Advanced Features

### Fast Mode (Beta)

Fast Mode prioritizes speed over detailed data. It returns a maximum of 21 results per query, ordered by proximity to the provided latitude and longitude.

#### Parameters for Fast Mode:
- `zoom`: Map zoom level (0–21).
- `radius`: Search radius in meters.
- `latitude`: Latitude coordinate.
- `longitude`: Longitude coordinate.

Example command:
```bash
./google-maps-scraper -fast-mode -radius 1000 -latitude 37.7749 -longitude -122.4194 -results fast_results.csv
```

---

### Extracted Data Points

Google Maps Scraper extracts the following data points by default:
```plaintext
input_id, link, title, category, address, open_hours, popular_times, website, phone,
plus_code, review_count, review_rating, reviews_per_rating, latitude, longitude,
cid, status, descriptions, reviews_link, thumbnail, timezone, price_range,
data_id, images, reservations, order_online, menu, owner, complete_address,
about, user_reviews, emails
```

*Note*: Email extraction is disabled by default. Use the `-email` parameter to enable it.

---

## Performance & Scalability

### Performance
- Handles up to **120 jobs per minute** with concurrency of 8.
- Each search generates 1 job plus results (e.g., 1,000 keywords × 16 results = 16,000 jobs). Expected runtime: ~2.5 hours.

### Scalability
For large-scale scraping:
1. Use the **Database Provider** with PostgreSQL to manage jobs.
2. Deploy the scraper on a **Kubernetes Cluster** for distributed execution.

---

## Telemetry

Anonymous usage statistics are collected to improve the tool. To opt-out, set the following environment variable:
```bash
DISABLE_TELEMETRY=1
```

---

## Sponsors & Integrations

### Evomi
- **Swiss Quality Proxy Provider**: Starting at $0.49/GB.
- 24/7 Expert Support.
- Low latency and high uptime.

👉 [Learn More](https://evomi.com?utm_source=github&utm_medium=banner&utm_campaign=gosom-maps)

---

### CapSolver
Automates CAPTCHA solving for efficient scraping. Supports reCAPTCHA V2, reCAPTCHA V3, and hCaptcha.  
👉 [Explore CapSolver](https://www.capsolver.com/?utm_source=github&utm_medium=banner_repo&utm_campaign=scraping&utm_term=giorgos)

---

### Google Maps API via ScraperAPI
Simplify Google Maps scraping with ScraperAPI for better success rates and scalability.  
👉 [Start your free trial today!](https://bit.ly/Scraperapi)

---

## Conclusion

Google Maps Scraper is a powerful tool for extracting business data at scale. Whether you're collecting contact info, reviews, or geospatial data, this scraper offers flexibility, scalability, and efficiency. By combining advanced features like proxy support and email extraction with scalable deployment options, you can tailor it to meet your unique data collection needs.

**Get started today** and unlock the full potential of Google Maps data!
