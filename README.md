# Flask Movie Collection App

This is a Flask web application that allows users to manage a collection of their favorite movies. The app uses Flask, SQLAlchemy for the database, WTForms for form handling, and Bootstrap for frontend styling. It also integrates with the Movie Database (TMDB) API to fetch movie details.

## Features

- **View Top 10 Movies**: Displays a list of the top 10 movies in your collection.
- **Add Movies**: Search for movies by title using the TMDB API and add them to your collection.
- **Update Movies**: Edit a movie's rating and review.
- **Delete Movies**: Remove movies from the collection.

## Dependencies

- **Flask**: A lightweight WSGI web application framework.
- **Flask-Bootstrap**: Integration of Bootstrap 5 with Flask for easier styling.
- **Flask-SQLAlchemy**: ORM extension for Flask to interact with the database.
- **WTForms**: Form validation and rendering for Flask.
- **Requests**: Library for sending HTTP requests to external APIs like TMDB.
- **TMDB API**: External movie database used to fetch movie details like title, release date, description, and poster.

## Code Structure

### 1. **Main Flask App (`main.py`)**

#### Imports

- **Flask and Extensions**: Import core Flask components, including `Flask`, `render_template`, `redirect`, `url_for`, `request`, and more.
- **SQLAlchemy**: Use SQLAlchemy to create models and interact with the SQLite database.
- **WTForms**: Import form handling utilities to create movie rating and search forms.
- **Requests**: Import `requests` to interact with the TMDB API.

#### App Setup

- The Flask app is initialized with a secret key and database URI for SQLite.
- Bootstrap is integrated for styling.
- TMDB API endpoints and key are defined for making API calls.

#### Database Setup

- **`Movie` Model**: Defines the `Movie` table with columns for `id`, `title`, `year`, `description`, `rating`, `ranking`, `review`, and `img_url`.
- **Database Initialization**: When the app starts, `db.create_all()` ensures that the `Movie` table is created if it doesn't exist.

#### Forms

- **`RateMovieForm`**: A form for editing the movie's rating and review.
- **`FindMovieForm`**: A form for searching for movies by title using the TMDB API.

#### Routes

- **Home Route (`/`)**: Displays the top 10 movies sorted by their rating. The ranking of each movie is dynamically calculated based on the list length.
  
    - **SQLAlchemy Query**: Fetches all movies from the database, orders them by rating, and dynamically updates their ranking.
  
- **Edit Route (`/edit`)**: Allows users to update the rating and review of a selected movie.

    - Uses `db.session.get()` to fetch the movie by its `id`. If the movie is not found, it returns a 404 error using `abort(404)`.

- **Add Movie Route (`/add`)**: A form for searching and adding movies to the collection using the TMDB API. The results are displayed on the `select.html` page, where users can select the movie they want to add.

- **Find Movie Route (`/find`)**: After selecting a movie from the search results, this route fetches the movie details from the TMDB API and adds it to the database.

- **Delete Movie Route (`/delete`)**: Deletes a movie from the collection. If the movie doesn't exist, it raises a 404 error.

### 2. **HTML Templates**

The app uses Flask's Jinja2 templating engine to render HTML files. The following templates are used:

- **`index.html`**: Displays the top 10 movies in a card layout. Each movie shows its ranking, poster, title, year, rating, review, and description. Each movie has options to update or delete.
  
- **`add.html`**: Contains a search form where users can input the title of the movie they want to add.

- **`edit.html`**: A form where users can update the rating and review of the selected movie.

- **`select.html`**: Displays a list of movies fetched from the TMDB API based on the search query. Users can select a movie to add to the collection.

### 3. **Error Handling**

- If a movie is not found in the database, the app raises a `404` error using Flask's `abort(404)` method.
  
### 4. **External API Integration**

The app uses the TMDB API to fetch movie data such as:

- **Movie Title**
- **Release Year**
- **Description**
- **Poster Image URL**

The TMDB API is queried in two scenarios:

- When the user searches for a movie title.
- When the user selects a movie to add to the collection.

### 5. **Custom Styling**

- The app is styled using Bootstrap classes. Each movie is displayed in a card format with a background image (movie poster), movie details on the back, and action buttons (update, delete).

### 6. **Database**

The app uses SQLite as the database. The schema consists of a single `Movie` table with the following fields:

- **id**: Integer (Primary Key)
- **title**: String (Movie Title)
- **year**: Integer (Release Year)
- **description**: String (Movie Description)
- **rating**: Float (User Rating)
- **ranking**: Integer (Movie Ranking in Top 10)
- **review**: String (User Review)
- **img_url**: String (Poster Image URL)

## How to Run

1. **Install Dependencies**:
    ```bash
    pip install -r requirements.txt
    ```

2. **Run the Flask App**:
    ```bash
    python main.py
    ```

3. **Open the App**:
    Open a browser and go to `http://127.0.0.1:5000/` to view the app.

## File Structure

- **main.py**: The main application logic.
- **templates/**: Directory containing the HTML templates.
    - **base.html**: The base template that other templates extend.
    - **index.html**: Displays the movie list.
    - **add.html**: Form for adding movies.
    - **edit.html**: Form for editing movie ratings and reviews.
    - **select.html**: Displays search results from TMDB API.
- **static/**: Contains static files like CSS and JavaScript.
    - **style.css**: Custom styles for the app.

## API Information

- **TMDB API Key**: The app uses the TMDB API to search for and fetch movie details. Ensure you have a valid API key, and replace it in the code where necessary.

## Future Improvements

- **User Authentication**: Implement user accounts to allow multiple users to maintain their own movie collections.
- **Advanced Search**: Add more search filters like genre, director, and actor.
- **Sorting Options**: Provide different ways to sort the movie list, such as by title, year, or user rating.
