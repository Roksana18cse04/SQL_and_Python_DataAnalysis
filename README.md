# Sakib Al Hasan Cricket Career Data Analysis

## Project Overview
This project focuses on collecting, analyzing, and visualizing Sakib Al Hasan's cricket career data. The main objectives are:

1. **Data Collection**: Scraping career statistics from [ESPNcricinfo](https://www.espncricinfo.com/cricketers/shakib-al-hasan-56143).
2. **Data Storage**: Saving the collected data in CSV format for further analysis.
3. **Data Analysis**: Using Python to analyze the data for insights.
4. **Dashboard Design**: Creating a dynamic dashboard using Power BI to visualize the analysis results.

---

## Steps and Tools Used

### 1. Data Collection
- **Description**: Web scraping is used to extract Sakib Al Hasan's career statistics (batting, bowling, and all-round performance).
- **Tools**: 
  - `Python`
  - Libraries: `requests`, `BeautifulSoup`, `pandas`
- **Output**: A CSV file named `shakib_al_hasan_stats.csv` containing the scraped data.

### 2. Data Analysis
- **Description**: Python is used to analyze the collected data. This involves:
  - Aggregating statistics for various formats (Test, ODI, T20I).
  - Identifying trends in performance (e.g., top matches, consistency).
- **Tools**:
  - `Python`
  - Libraries: `pandas`, `matplotlib`, `seaborn`

### 3. Dashboard Design
- **Description**: A Power BI dashboard is created to visualize insights such as:
  - Batting and bowling averages across formats.
  - Career progression over the years.
  - Top performances by opponent and venue.
- **Tools**:
  - `Microsoft Power BI`

---

## Prerequisites
1. **Python Setup**:
   - Install Python (>= 3.8).
   - Install required libraries:
     ```bash
     pip install requests beautifulsoup4 pandas matplotlib seaborn
     ```

2. **Power BI**:
   - Install Microsoft Power BI Desktop.

3. **Environment**:
   - Ensure internet access for web scraping.
   - Install a modern web browser for inspecting HTML structures.

---

## Usage

### Step 1: Run the Python Script
1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd <repository-folder>
   ```

2. Run the script to scrape data:
   ```bash
   python scrape_shakib_data.py
   ```

3. Verify the generated `shakib_al_hasan_stats.csv` file.

### Step 2: Analyze Data with Python
1. Open the `analyze_data.py` script.
2. Execute the script to generate plots and summary statistics:
   ```bash
   python analyze_data.py
   ```

### Step 3: Create Dashboard in Power BI
1. Load `shakib_al_hasan_stats.csv` into Power BI.
2. Design custom visualizations for insights.
3. Publish the dashboard for sharing or presentations.

---

## Folder Structure
```
project-root/
|-- README.md
|-- scrape_shakib_data.py     # Script for data scraping
|-- analyze_data.py           # Script for data analysis
|-- shakib_al_hasan_stats.csv # Collected data (output)
|-- powerbi_dashboard.pbix    # Power BI dashboard file
```

---

## Future Improvements
1. Automate data updates using a scheduler.
2. Integrate more advanced analysis using machine learning.
3. Add more players' data for comparative analysis.

---

## License
This project is open-source and available under the [MIT License](LICENSE).

---

## Author
[Your Name]  
[Your Contact Information]
