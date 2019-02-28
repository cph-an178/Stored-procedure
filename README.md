# Stored procedure and JSON
In this assignement we're working with MySQL stored procedures and JSON in MySQL 


## Exercise 1
Write a stored procedure `denormalizeComments(postID)` that moves all comments for a post (the parameter) into a json array on the post. 

**TODO**

## Exercise 2
Create a trigger such that new adding new comments to a post triggers an insertion of that comment in the json array from exercise 1.

**TODO**

## Exercise 3
Rather than using a trigger, create a stored procedure to add a comment to a post - adding it both to the comment table and the json array

**TODO**

## Exercise 4
Make a materialized view that has json objects with questions and its answeres, but no comments. Both the question and each of the answers must have the display name of the user, the text body, and the score.

**TODO**

## Exercise 5
Using the materialized view from exercise 4, create a stored procedure with one parameter `keyword`, which returns all posts where the keyword appears at least once, and where at least two comments mention the keyword as well.

**TODO**
