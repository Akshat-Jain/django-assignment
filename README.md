
# Django Fetch YouTube

[![Python Version](https://img.shields.io/badge/python-3.6.2-brightgreen.svg)](https://python.org)
[![Django Version](https://img.shields.io/badge/django-3.0.5-brightgreen.svg)](https://djangoproject.com)

## Table of Contents

* [Project Goal](#project-goal)
* [Running the Project Locally](#running-the-project-locally)
*  [Different endpoints](#different-endpoints)
* [Key Features](#key-features)
* [Video demonstration](#video-demonstration)
* [References](#references)
* [License](#license)

## Project Goal

To make an API to fetch latest videos sorted in reverse chronological order of their publishing date-time from YouTube for a given tag/search query in a paginated response.

## Running the Project Locally

1. First, clone the repository to your local machine:

```bash
git clone https://github.com/Akshat-Jain/django-assignment.git
```

2. Change the current directory to the project directory and install the requirements:

```bash
cd django-assignment
pip install -r requirements.txt
```

3. Setup migrations and database:

```bash
python manage.py makemigrations youtube
python manage.py migrate
```

4. Create an admin user:

```bash
python manage.py createsuperuser
```
Follow the prompts on the terminal to create an admin user.

5. We're almost there!
There's just one more thing we need to do!
We need to create a `keys.json` file in the root directory of the project where we will store our YouTube Data API Key which will be required to fetch the videos using that API.
```bash
touch keys.json
```

Now open this file with any text editor.
The structure of `keys.json` is required to be as follows:
```json
{
	"YOUTUBE_DATA_API_KEYS": {
		"key1": "Enter your API Key 1 here",
		"key2": "Enter your API Key 2 here",
		"key3": "Enter your API Key 3 here",
		"key4": "Enter your API Key 4 here",
	}
}
```
You can get an API key by following this [link](https://developers.google.com/youtube/v3/getting-started).
Make sure that you have atleast one API Key in this file with valid amount of quota left.

6. Finally, run the development server:

```bash
python manage.py runserver
```

The project will be available at **127.0.0.1:8000**.

## Different Endpoints

There are 3 endpoints in the project:
1. **`/admin`:** This allows user to login with the admin credentials and manage the project. The user can view the tables of the database and can also edit them directly in the provided interface.
2. **`/youtube/startFetching`:** This endpoint triggers a background asynchronorous job to fetch information of videos. This endpoint also supports an optional query parameter to take the search query:
```
http://localhost:8000/youtube/startFetching?searchQuery=Cricket
http://localhost:8000/youtube/startFetching?searchQuery=Westeros
```
The `searchQuery` is optional and defaults to `Game of Thrones` if not provided.

3. **`/youtube`:** This endpoint returns the stored video data in a paginated response sorted in descending order of published datetime. This endpoint also supports a query parameter to facilitate pagination as following:

```
http://localhost:8000/youtube/?page=1
http://localhost:8000/youtube/?page=2
.
.
http://localhost:8000/youtube/?page=pageNumber
```

You can find the demonstration of each of these in the [Demonstration](#video-demonstration) section.

## Key Features

1. Information about YouTube videos is being fetched asynchronously as a background task on regular intervals (10 seconds as per the current implementation).
2. Pagination support with latest published videos shown first.
3. Stores Video ID, Title, Description, Publishing Datetime and Thumbnails URLs in the database with indexing done on `Publishing Datetime` column in reverse chronological ordering for faster access.
4. Support for supplying multiple API keys so that if quota is exhausted on one, it automatically uses the next available key.
5. Renders the response in a custom implemented template.

## Video Demonstration

![](demo.gif)

## References

1. [YouTube Data v3 API](https://developers.google.com/youtube/v3/getting-started)
2. [Search API reference](https://developers.google.com/youtube/v3/docs/search/list)
3. [Django Official Documentation](https://docs.djangoproject.com/en/3.0/intro/)

## License

The source code is released under the [MIT License](https://github.com/Akshat-Jain/django-assignment/blob/master/LICENSE).