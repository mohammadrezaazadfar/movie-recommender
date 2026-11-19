# Content-Based Movie Recommender

A simple content-based movie recommender built with **pandas** on the [MovieLens](https://grouplens.org/datasets/movielens/) dataset. It is a learning project: everything is done with plain pandas operations (no scikit-learn), so each step is easy to follow.

## How it works

1. **Clean the data:** split the year from the title (`Toy Story (1995)` -> `Toy Story` + `1995`) with regex, and drop movies without a year.
2. **One-hot encode genres:** every genre becomes a 0/1 column (`str.get_dummies`). Movies with no genre are removed.
3. **Center the ratings:** subtract a neutral midpoint (5 on a 0-10 scale). Ratings above it count positively, ratings below it negatively, and the further from the midpoint, the stronger the effect.
4. **Build a taste profile:** multiply the genres of each movie the user rated by its centered rating, then sum per genre.
5. **Score every movie:** compute the **cosine similarity** between each movie's genre vector and the taste profile.
6. **Remove seen movies:** movies the user already rated are filtered out, then the top results are shown.

## Run it

```bash
git clone https://github.com/mohammadrezaazadfar/<repo-name>.git
cd <repo-name>
pip install -r requirements.txt
```

1. Download `movies.csv` from the MovieLens dataset (for example the `ml-latest-small` or `ml-latest` zip) and put it in the project folder, next to the notebook.
2. Open and run `content_base_RC.ipynb`:

```bash
jupyter notebook content_base_RC.ipynb
```

The dataset is not included in this repository because of the MovieLens license terms.

## Notes and limitations

- **Rating scale:** the example user rates movies from 0 to 10, so the midpoint is 5. The original MovieLens ratings go from 0.5 to 5, where the midpoint would be 2.75. Change `midpoint` if you use another scale.
- **Run the centering cell once:** it changes the `rating` column in place. Use *Restart & Run All* to run it again.
- **Only genres are used:** the model ignores popularity, quality, cast, and so on. Movies with many genres in common with the profile rank high, so a user's top-rated movie does not always dominate the results.
- **Not enough negative signal:** if the user only rates movies they like, no genre becomes negative, so nothing is pushed down.
- **Tiny example user:** six ratings are enough to show the idea, but not to make reliable recommendations.

## Ideas for next steps

- Use the real MovieLens `ratings.csv` and build profiles for many users.
- Add more features, such as tags or the release year.
- Evaluate the recommendations (for example by hiding some rated movies and checking if they come back).

## Data

MovieLens, provided by [GroupLens Research](https://grouplens.org/datasets/movielens/).
