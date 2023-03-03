# SQL CRUD
## Part 1
Restaurant table code:
```
CREATE TABLE restaurants(
    id INTEGER PRIMARY KEY,
    category TEXT NOT NULL,
    price TEXT NOT NULL,
    neighborhood TEXT NOT NULL,
    opening TEXT NOT NULL,
    closing TEXT NOT NULL,
    rate_stars INTEGER NOT NULL,
    kids TEXT NOT NULL
);
```
Review table code:
```
CREATE TABLE review(
    review_id INTEGER PRIMARY KEY,
    restaur_id INTEGER NOT NULL,
    reviews TEXT NOT NULL,
    rating INTEGER NOT NULL
);
```
This [restaurants data](data/restaurants.csv) was generated from [mockaroo.com](https://mockaroo.com/), which is 1000 rows of random restaurant datas.
Data set was imported with this code:
```
.mode csv
.import data/restaurants.csv restaurants
```
### Queries

1. Find all cheap restaurants in a particular neighborhood (pick any neighborhood as an example).
```
SELECT * FROM restaurants WHERE price = 'cheap' AND neighborhood = 'Lower East Side';
```
1. Find all restaurants in a particular genre (pick any genre as an example) with 3 stars or more, ordered by the number of stars in descending order.
```
SELECT * FROM restaurants WHERE rate_stars >= 3 AND category = 'Polish' ORDER BY rate_stars DESC;
```
1. Find all restaurants that are open now (see hint below).
```
SELECT * FROM restaurants WHERE strftime('%H:%M', 'now') BETWEEN opening AND closing;
```
1. Leave a review for a restaurant (pick any restaurant as an example).
```
INSERT INTO review (review_id, restaur_id, reviews, rating)
VALUES(1, 5, 'Food was ok, table was dirty.', 3);
```
1. Delete all restaurants that are not good for kids.
```
DELETE FROM restaurants WHERE kids = 'false';
```
1. Find the number of restaurants in each NYC neighborhood.
```
SELECT neighborhood, COUNT(*) FROM restaurants GROUP BY neighborhood;
```
## Part 2
Social media users table code:
```
CREATE TABLE user(
    id INTEGER PRIMARY KEY,
    email TEXT NOT NULL,
    password TEXT NOT NULL,
    handle TEXT NOT NULL
);
```
User post table code:
```
CREATE TABLE post(
    id INTEGER PRIMARY KEY,
    post_type TEXT NOT NULL,
    invisible TEXT NOT NULL,
    posts TEXT NOT NULL,
    user_from INTEGER NOT NULL,
    user_to INTEGER NOT NULL,
    time TEXT NOT NULL
);
```
This [user data](data/user.csv) and [post data](data/post.csv) were generated from [mockaroo.com](https://mockaroo.com/), and are 1000 rows of random users and post datas.

Data set for user was imported with this code:
```
.mode csv
.header on
.import data/user.csv user
```
Data set for post was imported with this code:
```
.import data/post.csv post
```
### Queries

1. Register a new User.
``` 
INSERT INTO user(email, password, handle) 
VALUES('jk6240@nyu.edu', 'djdfsnkne', 'jk6240');
```
1. Create a new Message sent by a particular User to a particular User (pick any two Users for example).
```
INSERT INTO post(post_type, invisible, posts, user_from, user_to, time)
VALUES('message', 'visible', 'hello how are you', '343', '231', '7:34');
```
1. Create a new Story by a particular User (pick any User for example).
```
INSERT INTO post(id, post_type, invisible, posts, user_from, user_to, time)
VALUES('1001, 'story', 'visible', 'time for a new mukbang', '342', '0', '12:33');
```
1. Show the 10 most recent visible Messages and Stories, in order of recency.
```
SELECT * FROM post WHERE invisible = 'visible' ORDER BY time DESC LIMIT 10;
```
1. Show the 10 most recent visible Messages sent by a particular User to a particular User (pick any two Users for example), in order of recency.
```
SELECT * FROM post WHERE post_type = 'message' AND user_from = '344' AND user_to = '343' ORDER BY time DESC LIMIT 10;
```
1. Make all Stories that are more than 24 hours old invisible.
```
UPDATE post SET invisible = 'invisible' WHERE post_type = 'story' AND ROUND((JULIANDAY('now') - JULIANDAY('2021-02-21 12:50:00')) * 24) > 24;
```
1. Show all invisible Messages and Stories, in order of recency.
```
SELECT * FROM post WHERE invisible = 'invisible' ORDER BY time DESC;
```
1. Show the number of posts by each User.
```
SELECT user_from, COUNT(*) FROM post GROUP BY user_from;
```
1. Show the post text and email address of all posts and the User who made them within the last 24 hours.
```
SELECT post.posts, user.email, user.id FROM post LEFT JOIN user ON user.id = post.user_from WHERE ROUND((JULIANDAY('now') - JULIANDAY('2021-02-21 12:50:00')) * 24) > 24;
```
1. Show the email addresses of all Users who have not posted anything yet.
```
SELECT user.email, user.id FROM user LEFT JOIN post ON user.id = post.user_from WHERE posts = '' GROUP BY user.id;
```