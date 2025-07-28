

# Earth911 Web Scraper

## Overview

This Python project uses Selenium and BeautifulSoup to automate the extraction of recycling business/location data from [Earth911’s search platform](https://search.earth911.com/).

It:

- Automates the search (“What” & “Zip Code”), sets the radius filter, handles popups, and scrapes all first-page results.
- For each result, visits the detail page and extracts:
    - Business Name
    - Last Update Date
    - Street Address (cleaned and concatenated)
    - A full, clean list of all materials accepted (from the detailed materials table)
- Cleans all output, removing invisible/strange Unicode, and exports everything to a clean CSV.

## Requirements

- **Python 3.7+**
- **Google Chrome** (latest stable recommended)
- **[ChromeDriver](https://chromedriver.chromium.org/downloads)** matching your installed Chrome version
- **Python Libraries**:
  - selenium
  - pandas
  - beautifulsoup4
  - (Optionally) lxml (for slightly faster BeautifulSoup parsing)

### Install Python dependencies

```bash
pip install selenium pandas beautifulsoup4
```

## Setup

1. **Install Google Chrome** if not already present.
2. **Download [ChromeDriver](https://chromedriver.chromium.org/downloads)** and ensure it matches your Chrome version. Make sure the `chromedriver` executable is in your system PATH, or specify its path in `webdriver.Chrome()` if needed.
3. **Clone the project/Copy the script** into your working folder.

## Usage

1. **Edit the script to set your desired search:**  
   Change these lines if you want other terms:

   ```python
   MATERIAL = "Electronics"
   PINCODE = "10001"
   ```

2. **Run the script:**

   ```bash
   python earth911_final_detail_table.py
   ```

3. **Output:**  
   At the end, you will get a CSV file:  
   ```
   earth911_cleaned_with_materialstable.csv
   ```
   Columns:
   - `business_name`
   - `last_update_date`
   - `street_address`
   - `materials_accepted_table` (a comma-separated list, always from the details table if present)

4. **What the script does:**
   - Loads the search page, fills in the search and pincode, clicks search, and applies the 100-mile radius filter.
   - Handles and closes any newsletter/pop-up windows at every stage, including after scrolling.
   - Extracts all first-page result links.
   - Opens each result in a new tab, extracts all required details (including all materials from the materials table), and cleans all output.
   - Closes all tabs/windows and saves a clean CSV file.

## Modifying or Extending

- To scrape different search material or pincode, simply change the `MATERIAL` and `PINCODE` variables.
- To crawl more than the first page (pagination), you’ll need to add code to click through the pager links at the bottom and repeat the extraction process.
- If you need more columns (e.g., phone number, distance, etc.), adjust the `extract_business_details` function to extract those fields.
- The script can be adapted for scheduling or integrated into a larger data-gathering workflow if needed.

## Notes and Tips

- **Popups:** The script is robust against Earth911’s popups but can be further tuned if their structure changes.
- **Unicode Cleaning:** All extracted text is normalized and cleaned of unwanted unicode, so your CSV is practical for analysis.
- **Performance:** The script opens each result in a new browser tab, which is stable but not the fastest. For larger datasets or multipage crawling, consider adding headless mode and parallelism.
- **Ethics:** Please respect [Earth911’s Terms of Service](https://earth911.com/about/terms-of-use/) and use this tool responsibly for research, not abuse.

## Troubleshooting

- **Selenium errors:** Make sure your ChromeDriver and Chrome browser versions match.
- **Popups not closing:** The script targets current popup structure; if it breaks, update the CSS selectors in `close_email_popup()`.
- **Data issues:** All outputs are forcibly cleaned, but if you encounter missed fields, review the `extract_business_details()` and `clean_text()` functions.

## License

For academic or research use. Not affiliated with nor endorsed by Earth911.

## Contact

Questions or ideas for improvement? Open an issue or contact @ priyanshu.arora.career@gmail.com .

**Happy recycling data extraction!**
