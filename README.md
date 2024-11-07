# Jumia iPhone Web Scraper and Data Analysis with Power Query

## Overview

This project demonstrates how to scrape product data from Jumia's iPhone section using Power Query and store the data in Excel. The project includes two main scripts: one for fetching data from multiple pages and another for extracting data from a single page. This approach allows users to perform analysis on product details such as prices, discounts, and ratings.

## Features

- **Web Scraping with Power Query**: Extracts data directly from the Jumia website.
- **Dynamic Page Fetching**: Capable of retrieving data from multiple pages by modifying a parameter.
- **Comprehensive Data Extraction**: Captures key product details, including names, prices, images, ratings, and discounts.
- **Excel Integration**: Stores the scraped data in Excel for further analysis.

## Project Details

### 1. Fetching Data from Multiple Pages

The script in this section demonstrates how to extract data from a specified number of pages on Jumia.


let
    // Number of pages to retrieve
    PageCount = 2, // Adjust to fetch more or fewer pages

    // Initialize an empty table
    CombinedData = Table.FromRecords({}),

    // Function to fetch data from each page
    GetDataFromPage = (Page) =>
    let
        // Modify link for page number
        Source = Web.BrowserContents("https://www.jumia.com.eg/mobile-phones/apple/?page=" & Text.From(Page)),
        #"Extracted Table From Html" = Html.Table(Source, 
            {{"Product Name", ".name"}, 
             {"Price", ".prc"}, 
             {"Image", ".img-c"}, 
             {"Additional Info", ".add"}, 
             {"Reviews", ".rev"}, 
             {"Stars", ".stars"}, 
             {"Old Price", ".old"}, 
             {"Discount", "._dsct"}, 
             {"Seller Info", ".mpg"}}, 
             [RowSelector=".prd"]
        )
    in
        #"Extracted Table From Html",

    // Collect data from each page
    PagesData = List.Transform({1..PageCount}, each GetDataFromPage(_)),
    CombinedDataResult = Table.Combine(PagesData)
in
    CombinedDataResult




### Feel free to edit or add more sections as needed for your GitHub repository.
