## 📁 About the Project
This project analyzes user behavior on a social media platform using SQL.

## 🛠️ Technologies
- MySQL Workbench
- SQL
- dbdiagram.io

## 🔍 Key Insights
- Most active user
- Most liked post
- Most popular posting day
- Comments per user

[comments.csv](https://github.com/user-attachments/files/20265355/comments.csv)[likes.csv](https://github.com/user-attachments/files/20265352/likes.csv)# Social-Media-Analysis-using-SQL
A simple SQL project analyzing social media
[Popular day for posting in week.csv](https://github.com/user-attachments/files/20265337/Popular.day.for.posting.in.week.csv)(popular day of posting on social media)

[social_media_analytics sql.csv](https://github.com/user-attachments/files/20265338/social_media_analytics.sql.csv)(Daily engagement trends)

[Most active user.csv](https://github.com/user-attachments/files/20265341/Most.active.user.csv)(Most active user)

[engagement rate per user.csv](https://github.com/user-attachments/files/20265342/engagement.rate.per.user.csv)(engagement rate per user)

[Screenshot 2025-05-17 195728.pdf](https://github.com/user-attachments/files/20265343/Screenshot.2025-05-17.195728.pdf)(ER DIAGRAM)

📦 Dataset
Synthetic dataset with 1000+ rows across Users, Posts, Likes, and Comments.
[users.csv](https://github.com/user-attachments/files/20265350/users.csv) (user data)
[posts.csv](https://github.com/user-attachments/files/20265351/posts.csv) (posts data)
[likes.csv](https://github.com/user-attachments/files/20265356/likes.csv) (likes data)
[comments.csv](https://github.com/user-attachments/files/20265364/comments.csv) (comments data)


**CODE FOR THE FOLLOWING**
CREATE DATABASE SMA;
USE SMA;

/*
BASIC UNDERSTANDING
1. USER
	- USER_ID(PK)
    - USERNAME
    - AGE
    - GENDER
    - COUNTRY
2. POSTS
	- POST_ID(PK)
    - USER_ID(FK)
    - POST_CONTENT
    - POST_DATE
3. LIKES
	- LIKE_ID(PK)
    - POST_ID(FK)
    - USER_ID(FK)
    - LIKE_DATE
4. COMMENTS
	- COMMENT_ID(PK)
    - POST_ID(FK)
    - USER_ID(FK)
    - COMMENT_TEXT
    - COMMENT_DATE
*/

-- CREATING TABLES

-- USER
CREATE TABLE USER1 (
    USER_ID INT PRIMARY KEY,
    USERNAME VARCHAR(100),
    AGE INT,
    GENDER VARCHAR(6),
    COUNTRY VARCHAR(50)
);
SELECT * FROM USER1

-- POSTS
CREATE TABLE POSTS(
	POST_ID INT PRIMARY KEY,
    USER_ID INT,
    POST_CONTENT TEXT,
    POST_DATE DATE,
    FOREIGN KEY(USER_ID) REFERENCES USER1(USER_ID)
    );
    SELECT * FROM POSTS
    
-- LIKES
	CREATE TABLE LIKES (
    LIKE_ID INT PRIMARY KEY,
    POST_ID INT,
    USER_ID INT,
    LIKE_DATE DATE,
    FOREIGN KEY (POST_ID)
        REFERENCES POSTS (POST_ID),
    FOREIGN KEY (USER_ID)
        REFERENCES USER1 (USER_ID)
);
SELECT * FROM LIKES;

-- COMMENTS
CREATE TABLE COMMENTS (
    COMMENT_ID INT PRIMARY KEY,
    POST_ID INT,
    USER_ID INT,
    COMMENT_TEXT TEXT,
    COMMENT_DATE DATE,
    FOREIGN KEY (POST_ID)
        REFERENCES POSTS (POST_ID),
    FOREIGN KEY (USER_ID)
        REFERENCES USER1 (USER_ID)
);
SELECT * FROM COMMENTS

-- BEFORE THIS IMPORT THE DATA SET INTO SQL WORK BENCH USING WIZARDS

-- LOADING DATA

SELECT * FROM USER1 
SELECT * FROM POSTS
SELECT * FROM LIKES
SELECT * FROM COMMENTS LIMIT 0, 50000;

# analysis tasks

**ANALYTICAL TASKS**

MOST ACTIVE USER
	SELECT USER_ID, COUNT(*) AS POST_COUNT
    FROM POSTS
    group by USER_ID
    ORDER BY POST_COUNT DESC;
    LIMIT 1;
    
  POST WITH MOST LIKES
    SELECT POST_ID, COUNT(*) AS LIKE_COUNT
    FROM LIKES
    GROUP BY POST_ID
    ORDER BY LIKE_COUNT DESC
    LIMIT 1
    
  MOST POPULAR DAY OF POSTING IN A WEEK
    SELECT DAYNAME(POST_DATE) AS DAY_OF_WEEK, COUNT(*) AS POST_COUNT
    FROM POSTS
    GROUP BY DAY_OF_WEEK
    ORDER BY POST_COUNT DESC
    
  NUMBER OF COMMMENT FROM USER
    SELECT USER_ID , COUNT(*) AS COMMENT_COUNT
    FROM COMMENTS
    GROUP BY USER_ID
    ORDER BY COMMENT_COUNT DESC
    LIMIT 1;
    
  ENGAGEMENT RATE PER USER
    SELECT u.username,
       COUNT(DISTINCT p.post_id) AS posts,
       COUNT(DISTINCT l.like_id) AS likes,
       COUNT(DISTINCT c.comment_id) AS comments
FROM Users u
LEFT JOIN Posts p ON u.user_id = p.user_id
LEFT JOIN Likes l ON p.post_id = l.post_id
LEFT JOIN Comments c ON p.post_id = c.post_id
GROUP BY u.username
ORDER BY likes + comments DESC;

-- DAILY ENGAGEMENT TREND
SELECT post_date,
       COUNT(DISTINCT p.post_id) AS posts,
       COUNT(DISTINCT l.like_id) AS likes,
       COUNT(DISTINCT c.comment_id) AS comments
FROM Posts p
LEFT JOIN Likes l ON p.post_id = l.post_id
LEFT JOIN Comments c ON p.post_id = c.post_id
GROUP BY post_date
ORDER BY post_date;

