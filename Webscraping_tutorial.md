# Web Scraping Tutorial

Welcome to the **Web Scraping Tutorial**! This tutorial will guide you through the basics of web scraping using Python. By the end of this tutorial, you'll be able to extract data from websites and process it for your own use.

## Table of Contents
- [Course 1 - Introduction to Web Scraping](#course-1---introduction-to-web-scraping)
  - [Overview of the Course](#overview-of-the-course)
  - [The Classic Web Scraping Example](#the-classic-web-scraping-example)
- [Course 2 - Fetching Web Pages](#course-2---fetching-web-pages)
  - [Setting Up Your Environment](#setting-up-your-environment)
  - [Fetching Web Pages with Requests](#fetching-web-pages-with-requests)
- [Course 3 - Parsing HTML Content](#course-3---parsing-html-content)
  - [Parsing with BeautifulSoup](#parsing-with-beautifulsoup)
  - [Example: Extracting Titles](#example-extracting-titles)
- [Course 4 - Extracting and Cleaning Data](#course-4---extracting-and-cleaning-data)
  - [Extracting Hyperlinks](#extracting-hyperlinks)
  - [Cleaning Extracted Data](#cleaning-extracted-data)
- [Course 5 - Saving Data for Analysis](#course-5---saving-data-for-analysis)
  - [Saving Data to a CSV File](#saving-data-to-a-csv-file)
- [Conclusion](#conclusion)

---

## Course 1 - Introduction to Web Scraping

### Overview of the Course
In this section, you'll get an overview of what web scraping is and how you can use it to extract data from websites.

- Course 1: Setting up the environment
- Course 2: Fetching web pages
- Course 3: Parsing HTML content
- Course 4: Extracting and cleaning data
- Course 5: Saving data for analysis

### The Classic Web Scraping Example
We’ll start with a simple web scraping task to give you a feel for the process.

```python
import requests
from bs4 import BeautifulSoup

url = 'https://example.com'
response = requests.get(url)
page_content = response.text

soup = BeautifulSoup(page_content, 'html.parser')
print(soup.title.text)
```
This script fetches a web page and prints the title.

---

## Course 2 - Fetching Web Pages

### Setting Up Your Environment
Before starting with web scraping, ensure you have the necessary Python libraries installed:

```bash
pip install requests beautifulsoup4 pandas
```

### Fetching Web Pages with Requests
To scrape data from a website, the first step is to fetch the web page content:

```python
url = 'https://example.com'
response = requests.get(url)
page_content = response.text
```
This code fetches the HTML content of the page at the specified URL.

---

## Course 3 - Parsing HTML Content

### Parsing with BeautifulSoup
After fetching the web page, you need to parse the HTML content to extract the required information:

```python
from bs4 import BeautifulSoup

soup = BeautifulSoup(page_content, 'html.parser')
```
This creates a BeautifulSoup object that allows you to navigate and search through the HTML structure.

### Example: Extracting Titles
Let's extract all the titles from the page using the `<h1>` tags:

```python
titles = soup.find_all('h1')
for title in titles:
    print(title.get_text())
```
This code snippet extracts all the `<h1>` tags from the page and prints the text content.

---

## Course 4 - Extracting and Cleaning Data

### Extracting Hyperlinks
Web scraping often involves extracting hyperlinks from a page:

```python
links = soup.find_all('a')
for link in links:
    print(link.get('href'))
```
This example extracts all the hyperlinks from the page.

### Cleaning Extracted Data
Sometimes, the extracted data needs to be cleaned up before it's useful. Here's an example of cleaning a price value:

```python
def clean_price(price):
    return float(price.replace('$', '').replace(',', ''))
```
This function removes unwanted characters from the price string and converts it to a float.

---

## Course 5 - Saving Data for Analysis

### Saving Data to a CSV File
After extracting and cleaning your data, you’ll often want to save it for further analysis. Here’s how you can save your data using pandas:

```python
import pandas as pd

data = {'Title': titles, 'Link': links}
df = pd.DataFrame(data)
df.to_csv('scraped_data.csv', index=False)
```
This code saves the scraped data to a CSV file named 'scraped_data.csv'.

---

## Conclusion

Congratulations! You've completed the Web Scraping Tutorial. In this tutorial, you've learned how to:

- Set up your environment for web scraping
- Fetch and parse web pages
- Extract and clean data
- Save the data for further analysis

With these skills, you're ready to start scraping data from websites and using it in your own projects. Happy scraping!
