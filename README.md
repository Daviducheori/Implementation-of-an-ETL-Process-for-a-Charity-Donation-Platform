# Implementation of an ETL process for a charity donation platform

### Table of Content
- [Project Overview](#project-overview)
- [Data Source](#data-source)
- [Data Description](#data-description)
- [ETL Process Development](#etl-process-development)
- [Extraction](#extraction)
- [Transformation](#transformation)
- [Loading](#loading)

### Project Overview
This project describes the an Extract-Transform-Load(ETL) process for a charity donation platform. The project's outcome leads to the development of a fully functional and transparent platform for charitable donations that organise charities according to a variety of causes, subcategories, and geographical areas. Individuals will find it relatively easy to search for charities based on their region and preferred causes of interest. The ETL process was employed to clean and transform the raw data into a usable format for the database. Python scripts were used to implement the ETL process, with various libraries such as pandas and NumPy were used to perform the required data transformations.The project starts by accessing the target data. Each data source is transmitted into the transformation function. The purpose of the transformation function is to modify the data to make it more suitable for persistence within the database. 

### Data Source
The data used for this project were the datasets of all the Charities in the different countries of the United Kingdom and they were retrieved from the Charity Commission for England and Wales, the Scottish Charity Regulator (OSCR) and the Charity Commission for Northern Ireland. 

### Data Description
The datasets varied in their attributes and number of records. The Northern Ireland dataset consists of 7,053 entries and 20 attributes, the Scotland dataset consists of 25,429 entries and 36 attributes and the England and Wales dataset consists of 380,659 entries and 34 attributes. The datasets encompass several variables; However, it should be noted that certain variables required for the project objectives, particularly those necessary to fulfil the requirements of the organisation for the platform, were not present in the original datasets. As a result, new variables were generated from existing variables to meet the specific needs and requirements of the organisation's charity donation platform.

### Programming Language
- Python

## ETL Process Development 

### Extraction
The first step in the ETL process was data extraction. The Python code downloads data from the Charity Commissions of Northern Ireland, England and Wales and Scotland, saves it in a local directory named 'charity_data', and extracts the data from zip files to obtain CSV and JSON files using libraries such as 'requests', 'io', 'zipfile', and 'os'. The 'os' library is used to generate a local directory named 'charity_data' if it does not already exist, where the downloaded data and extracted files will be saved. The 'requests' library is used for making HTTP requests to the respective URLs, also to download the zip files containing the data. The 'io' and 'zipfile' libraries are then used to handle the zip files and extract their contents, in which one was a JSON file and two were CSV files containing the charity data. The downloaded data is then read using the 'pandas' library.

- Loading JSON Data into a Pandas DataFrame:
The data from one of the data sources was in a JSON format, the above code segment loads the JSON file into a Pandas DataFrame in Python, as part of the data extraction step in the ETL process.

### Transformation

The transformation process was carried out to standardise the data across the different datasets and convert data into a suitable format for the database. The datasets were subjected to data cleansing, where Python scripts were used to clean and remove any inconsistencies, errors, or irrelevant data.

Note: The requirements were gathered through meetings to ensure that the data is transformed according to the needs and specifications of the organisation and charity donation platform.

- Selecting Registered Charities:
The selection of registered charities based on the “charity registration status” attribute was performed. This involved filtering the datasets to include only those entries with a registered charity status, while excluding any non-registered charities. The filtered dataset containing only registered charities was then used for further data processing and analysis.

- Data Cleaning and Transformation of Charity Contact Address Data for England and Wales:
The code performs data cleaning and transformation operations on the charity contact address data for England and Wales. It concatenates address columns and removes extra commas and whitespaces. The original address columns are dropped from the DataFrame after cleaning. These operations prepare the data for further analysis and integration with other data sources in the ETL process.

- Classification of Charities into Causes and Subcategories:
This approach of classifying the Charities into causes and subcategories will enable easy filtering of Charities by donors. It will allow users to easily locate Charities that align with their interests and provide them with additional information about the charity's operations and beneficiaries.
I performed exploratory analysis on the data for the Charities in the different countries in the United Kingdom. The datasets contained attributes that relate to the nature of the Charity's work and the beneficiaries it serves. England, Wales, and Northern Ireland had the following related attributes:
- A. What the Charity does
- B. How the Charity works
- C. Who the Charity helps
  
#### Scotland on the other hand had the following attributes:
- A. Purposes
- B. Beneficiaries
- C. Objectives
  
#### The attributes of "What the Charity Does" for the cause classification and "How the Charity Works" and "Who the Charity Helps" for the subcategory classification were used to divide charities into causes and subcategories for England, Wales, and Northern Ireland. The reason for this is that the "what" factor is often the primary reason why people choose to support a Charity. The "how" and "who" factors are also important but may be secondary to the "what" factor.

The classification of Charities into causes and subcategories for Scotland was based on the attribute "Purposes" for the cause classification and the attributes "Beneficiaries” and "Objectives" for the subcategory classification. The resulting classification was added as additional attributes in the dataset.

- Development of Comprehensive List of Causes and Subcategories for the Platform: 
After analysing the classifications of Charities from the three different data sources, taking into consideration the nature of their work and the beneficiaries they serve, and studying existing Charity donation platforms, a comprehensive list of causes and subcategories was generated. This list was carefully aligned with the identified causes and subcategories, considering the nuances of each data source. A number of 22 causes and 167 subcategories were created. A list of this is embedded in the appendix. This meticulous approach ensured that the list of causes and subcategories on the platform accurately reflected the diverse range of charitable work and beneficiaries, providing donors with a comprehensive and meaningful selection of causes to support.

- Creating a Causes Attribute Based on The Predefined List:
The code classifies Charities into different causes based on a predefined list of causes using a group dictionary. The dictionary group_dict contains cause categories as keys and corresponding keywords related to those causes as values. The code uses a function called check_groups to check if the description of a Charity contains any of the keywords from the group_dict dictionary. If a keyword is found in the description, the corresponding cause category is appended to a list of groups. The check_groups function uses regular expressions to search for whole words in a case-insensitive manner. The lambda function is used to apply the check_groups function to each row of the respective columns and concatenate the causes into a comma-separated string using ', ‘. join(set(check_groups(x, group_dict))). The set function is used to remove duplicate causes, if any, and convert the causes to a set before joining them into a string.
Overall, this allows the dataset to be classified into causes based on the predefined keywords in the group dictionary. Any text that contains one or more of these keywords will be classified under the corresponding cause.

- Creating a Subcategory Attribute Based on The Predefined List:
The methodology employed for categorizing causes was also applied to create subcategory attributes, leveraging the predefined list. This approach ensured that charities could be further classified into more specific subcategories, aligning with the identified subcategories.

- Obtaining the Latitude and Longitude Coordinates:
Geocoding of the charities' latitude and longitude was required to enable location-based searching on the platform. This involved extracting the postcodes from the "address" column of some of the dataset that did not have a postcode attribute, and then using this information to obtain the corresponding latitude and longitude coordinates.

- Generating Postcode Using the Address Attribute for Northern Ireland:
The code segment is extracting postcodes from the "address" column. The code uses regular expressions (regex) to define a pattern for postcodes, which is stored in the variable "postcode_pattern". The “. apply()" function is then used to apply the regex pattern to each value in the "Public address" column. If a match is found, the "re.search(postcode_pattern, x).group()" statement extracts the matched postcode from the "address" value, and if no match is found, "None" is returned. The extracted postcodes are stored in a new column called "postcode".

- Obtaining the Latitude and Longitude Using Postcodes:
The latitude and longitude coordinates of each Charity in the dataset were obtained using the “postcode” attribute. The Geocoding API provided by Google Maps was used to obtain latitude and longitude coordinates for each Charity in the dataset. Python requests library was used to send requests to the Geocoding API and parse the JSON response. The resulting coordinates were added as additional attributes in the dataset. The aim of this process was to extract location information for the Charities in the dataset, which could be used for data visualisation and analysis purposes.

- Generating City Attribute:
The creation of a city attribute was also deemed necessary in order to facilitate location-based search of Charities by individuals based on cities.

- Generating a City Attribute Using for England and Wales Using the Address:
The code creates a function called extract_city that takes an address as input. It then defines a list of cities in England and Wales. The function iterates over the list of cities and checks if each city is present in the input address (case-insensitive). If a city is found in the address, it is returned as the extracted city. If no city is found, np.nan (NumPy's "not a number" value) is returned. The function is then applied to the "address" column of the DataFrame using the apply method. This creates a new column called "City" in the DataFrame, where the extracted city values are stored.

- Generating a City Attribute Using the Postcode:
The code defines two functions. The first function, get_city_from_postcode, retrieves city information from a given postcode using the Postcodes.io API. The second function is_null_or_empty, checks if a value is null or empty. The code then applies these functions to a DataFrame called using the apply() method. It uses is_null_or_empty() to generate a boolean mask for rows where the "City" column is null or empty. The "City" column of these rows is then filled with city information obtained from postcodes using the get_city_from_postcode() function.

- Generating Country Attribute:
The addition of a "Country" attribute was necessary to enable individuals to search for charities based on location, specifically Countries.

- Web Scraping:
In order to offer potential donors more information about the Charities, it was necessary to extract the logos and brief descriptions of each Charity. To achieve this, a web scraping approach was employed to extract the relevant resources from the Charities' websites. This enabled the inclusion of these resources in the charity donation application, thereby providing users with a more comprehensive understanding of each Charity's identity and mission.

- Web Scraping for Charity Logos:
This code extracts logo image URLs from the websites of Charities listed in a dataset. The code uses the BeautifulSoup library, which is a popular Python library for web scraping, to parse the HTML content of the Charity websites and extract logo image tags. It iterates through the rows of the dataset, extracting the website URL and registered Charity number for each Charity. It checks if the website URL is NaN or if it lacks a protocol scheme (i.e., "http://" or "https://"). If it doesn't have a protocol scheme, it adds "https://" as the prefix. It makes a GET request to the Charity's website using the requests library, with a timeout value set to 5 seconds to avoid long delays. It gets the HTML content of the response and parses it using BeautifulSoup with the 'html.parser' parser. It looks for the logo image tag in different sections of the HTML content, such as the header and body, and also by class or ID name. If a logo image tag is found, it extracts the source URL of the logo image from the 'src' attribute. It replaces any spaces in the logo URL with '%20' to ensure correct URL formatting. It stores the logo URL in the 'Logo Filename' column of the dataset for the corresponding Charity. It handles timeouts, connection errors, and other exceptions by printing error messages and continuing to the next row in the dataset.

- Web Scraping for Charity Description:
This code uses the requests and BeautifulSoup libraries to search for "About Us" pages on websites listed in a DataFrame for the Charities. It defines a function called search_about_us which takes a URL as input and attempts to access the URL and parse its HTML content using BeautifulSoup. It then looks for links with "about" or "about us" in their href attribute, decodes the URL-encoded strings, and retrieves the content of the linked page. If the page contains text, it is stored in the 'description' column of the DataFrame for the corresponding charity. The code iterates through the rows of the DataFrame and calls the search_about_us function for each website URL, storing the extracted information in the DataFrame. Error handling is included for exceptions such as failed requests and Unicode decoding errors.

### Loading

- Selecting and Renaming the Required Attributes:
To determine the selected attributes that should be extracted from the dataset and loaded into the database, a consultation process was conducted with the stakeholders. The stakeholders identified the key variables and data points that are most relevant to the goals and objectives of the charity donation platform.
Based on their input, a list of essential attributes was compiled, which included variables such as registration_number, charity_name, postcode, website, latitude, longitude, country, city, address, logo_filename, causes, subcategory. These attributes were deemed critical to achieving the platform's aims of providing users with detailed information about charities and facilitating donations to specific causes. The selected attributes were used as a basis for the extraction and transformation process above. They were also renamed for consistency in the naming for all the datasets.

