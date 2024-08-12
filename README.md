
# Project Definition and Dataset Selection

## Project Definition:

1. **Objective:** - To Develop a semantic book recommendation system that suggests books based on user preferences and semantic analysis of book content and reviews.
2. **Scope:** - Utilize Google Books API to retrieve book data and additional datasets from Kaggle, including Amazon book reviews and Amazon books datasets.
3. **What We Are Planning to Do:**
- Collect and preprocess data from multiple sources.
- Apply semantic analysis to understand the content and context of books and reviews.
- Develop a recommendation algorithm that leverages semantic relationships to suggest relevant books to users.

## Dataset Selection:

- **Google Books API:** Use the `requests` library in Python to fetch book data from the Google Books API and convert it into a dataframe using `Pandas` library.
- **Amazon Books Dataset:** Download and preprocess the Amazon books dataset from Kaggle with the help of kaggle API.
- **Amazon Book Reviews Dataset:** Download and preprocess the Amazon book reviews dataset from Kaggle with the help of kaggle API.

>[!Note]
> There are multiple files inside the Amazon Books dataset and Amazon book reviews dataset. We only going to use books_df.csv and books_data.csv files from Amazon books dataset and Amazon book reviews respectively.
