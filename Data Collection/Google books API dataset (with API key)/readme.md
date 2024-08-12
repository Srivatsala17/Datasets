# Google Books API Dataset with API Key

1. **Importing Libraries**:
   ```python
   import requests
   import pandas as pd
   ```
   - **Purpose**: Import the necessary libraries. `requests` is used for making HTTP requests, and `pandas` is used for data manipulation and analysis.

2. **API Key and Query List**:
   ```python
   api_key = 'AIzaSyCQ251arD_Pa8S8_cLKxmDW7Yy6a19HqAI'
   queries = ['Books', 'Fiction', 'Non-fiction', 'Science', 'History', 'Technology', 'Art', 'Biography', .......]
   ```
   - **Purpose**: Define the API key for authentication and a list of search queries to fetch books related to different topics.

> [!NOTE]
> Create API Key by using the link `https://console.cloud.google.com/apis/library/books.googleapis.com`
> Click Enable API and click on Add new credentials to create the API key

3. **Initialize an Empty List**:
   ```python
   all_books = []
   ```
   - **Purpose**: Create an empty list to store all the book data retrieved from the API.

4. **Loop Through Queries**:
   ```python
   for query in queries:
       start_index = 0
       while len(all_books) < 100000:
           url = f'https://www.googleapis.com/books/v1/volumes?q={query}&key={api_key}&maxResults=40&startIndex={start_index}'
           response = requests.get(url)
           data = response.json()
           if 'items' not in data:
               break
           all_books.extend(data['items'])
           start_index += 40
           if len(data['items']) < 40:
               break
   ```
   - **Purpose**: Loop through each query to fetch book data from the Google Books API.
     - **Inner Loop**: Continue fetching data until the total number of books reaches 100,000 or no more data is available.
     - **URL Construction**: Construct the API request URL with the query, API key, and pagination parameters.
     - **API Request**: Make the request and parse the JSON response.
     - **Data Check**: If no items are found in the response, break the loop.
     - **Data Storage**: Add the retrieved items to the `all_books` list.
     - **Pagination**: Increment the `start_index` to fetch the next set of results.
     - **Break Condition**: Stop if fewer than 40 items are returned, indicating no more data.

5. **Convert to DataFrame**:
   ```python
   df = pd.json_normalize(all_books)
   ```
   - **Purpose**: Convert the list of book data into a pandas DataFrame for easier manipulation and analysis.

6. **Reindex DataFrame**:
   ```python
   columns = ['volumeInfo.title', 'volumeInfo.ISBN', 'volumeInfo.authors', 'volumeInfo.publisher', 'volumeInfo.publishedDate', .......]
   df = df.reindex(columns=columns, fill_value='')
   ```
   - **Purpose**: Reindex the DataFrame to include only the specified columns, filling any missing values with an empty string.

7. **Convert Lists to Tuples**:
   ```python
   df['volumeInfo.authors'] = df['volumeInfo.authors'].apply(lambda x: tuple(x) if isinstance(x, list) else x)
   ```
   - **Purpose**: Convert lists in the `volumeInfo.authors` column to tuples to make them hashable, which is necessary for removing duplicates.

8. **Remove Duplicates**:
   ```python
   df.drop_duplicates(subset=['volumeInfo.title', 'volumeInfo.authors'], inplace=True)
   ```
   - **Purpose**: Remove duplicate entries based on the combination of book title and authors to ensure each book is unique.

9. **Save to CSV**:
   ```python
   df.to_csv('books_data13.csv', index=False)
   ```
   - **Purpose**: Save the cleaned and processed DataFrame to a CSV file named `books_data13.csv` without including the index.
     
# Challenges faced

* Initially the dataset was returning only 40-70 records. This might be because the because the Google Books API has a default limit on the number of results it returns per request.
* In order to get more records, the number of values to be returned is mentioned explicitly and just to check if there are more than 40 records available, if condition of len(data)<40 is used.
* Since the data is stored as list, initially the code showed `TypeError: unhashable type: 'list'`. So had to convert them into a tuple.
* In order to make sure that same books are not repeated, duplicates are removed for title and author.
* By running this code more than 150000 records will be displayed.

