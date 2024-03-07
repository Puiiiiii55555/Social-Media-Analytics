-- Count the total number of posts
SELECT COUNT(*) AS total_posts
FROM social_media_posts;

-- Count the number of posts per user
SELECT user_id, COUNT(*) AS num_posts
FROM social_media_posts
GROUP BY user_id
ORDER BY num_posts DESC;

-- Count the number of likes per post
SELECT post_id, COUNT(*) AS num_likes
FROM likes_table
GROUP BY post_id
ORDER BY num_likes DESC;

-- Identify the most popular posts (based on likes)
SELECT p.post_id, p.content, COUNT(l.user_id) AS num_likes
FROM social_media_posts p
LEFT JOIN likes_table l ON p.post_id = l.post_id
GROUP BY p.post_id, p.content
ORDER BY num_likes DESC
LIMIT 10;

-- Calculate the average number of likes per post
SELECT AVG(num_likes) AS avg_likes_per_post
FROM (
    SELECT post_id, COUNT(*) AS num_likes
    FROM likes_table
    GROUP BY post_id
) AS likes_per_post;

-- Identify the most active users (based on number of posts)
SELECT user_id, COUNT(*) AS num_posts
FROM social_media_posts
GROUP BY user_id
ORDER BY num_posts DESC
LIMIT 10;

-- Identify the users with the highest average number of likes per post
SELECT user_id, AVG(num_likes) AS avg_likes_per_post
FROM (
    SELECT p.user_id, p.post_id, COUNT(l.user_id) AS num_likes
    FROM social_media_posts p
    LEFT JOIN likes_table l ON p.post_id = l.post_id
    GROUP BY p.user_id, p.post_id
) AS likes_per_post
GROUP BY user_id
ORDER BY avg_likes_per_post DESC
LIMIT 10;
