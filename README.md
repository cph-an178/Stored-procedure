# Stored procedure and JSON
In this assignement we're working with MySQL stored procedures and JSON in MySQL 

## Setup

1. Move the xml files from Archive.org to MySQL files directory. To find the MySQL files directory run this command in MySQL `SHOW VARIABLES LIKE "secure_file_priv";`. This should output something like this:
    ```
    +------------------+-----------------------+
    | Variable_name    | Value                 |
    +------------------+-----------------------+
    | secure_file_priv | /var/lib/mysql-files/ |
    +------------------+-----------------------+
    1 row in set (0.00 sec)

    ```
    My path is `/var/lib/mysql-files/` but yours might differ.

2. When the files are in the folder, change the path on `load xml infile` on line 93 to 121 in `create-database.sql` 

    - Evt. change the database name 

3. Run the script

## Exercise 1
Write a stored procedure `denormalizeComments(postID)` that moves all comments for a post (the parameter) into a json array on the post. 


```sql
CREATE PROCEDURE denormalizeComments(IN p_postId INT)
BEGIN
    UPDATE posts SET Comments = (
        select JSON_ARRAYAGG(JSON_OBJECT('id', Id, 'score', Score, 'text', Text, 'creationDate', CreationDate, 'userId', userId)) 
        from comments 
        where PostId = p_postId
    ) 
    WHERE Id = p_postId;
END;
```

## Exercise 2
Create a trigger such that new adding new comments to a post triggers an insertion of that comment in the json array from exercise 1.

```sql
CREATE TRIGGER after_comments_insert 
    AFTER INSERT ON comments
    FOR EACH ROW 
BEGIN
    CALL denormalizeComments(NEW.PostId);
END
```
## Exercise 3
Rather than using a trigger, create a stored procedure to add a comment to a post - adding it both to the comment table and the json array

```sql
CREATE PROCEDURE createComment(IN p_Id INT, IN p_PostId INT, IN p_Score INT, IN p_Text text, IN p_CreationDate DATETIME, IN p_UserId INT)
BEGIN
    INSERT INTO comments (Id, PostId, Score, Text, CreationDate,UserId) values (p_Id, p_PostId, p_Score, p_Text, p_CreationDate, p_UserId);
    CALL denormalizeComments(p_postId);
END;
```
