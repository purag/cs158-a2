# CSE 158 Assignment 2
For our assignment, we used the "hetrec" last.fm dataset.

## Baseline Model, acc ~ 57%
Return true if this artist is one of the top 50 artists in our training set.

## Models
All of the models differ only in the fourth case (the `else`) of the if statement in `m*_predict`. This is the case in which we have seen a user and an artist in training, but have not seen a prior listen between them. Each of the descriptions below will only discuss that case.

All of the models take as input a `userID` (as `uid`) and an `artistID` (as `aid`).

Below, `acc` refers to the best accuracy we found on the validation set (by tuning the hyperparameters).

### Model 1, acc ~ 68%
Return true if any of this user's friends have listened to this artist.

### Model 2, acc ~ 81%
Return true if this user is similar to *any* of the users that have listened to this artist. ("Similar" means their Jaccard similarity [based on the artists they've listened to] exceeds the threshold of 0.4, which we found experimentally via tuning the model based on results on the validation set).

### Model 3, acc ~ 74%
Return true if the any of the user's top tags are in the artist's tags (user top tags = the set of tags attributed to any artist by this user, reverse sorted, and sliced). Best accuracy was observed when taking the top 16 of the user's tags.

In this model, we sorted tags by the number of times they were used for a given user or artist.

### Model 4, acc ~ 68%
Same as above, except in this model, we sorted tags by the number of listens associated with them (for a given user/artist pair, we had both the number of listens as well as a collection of tags for the pair; increment the tag's frequency by the number of listens).

### Model 5, acc ~ 72%
Same as **Model 2**, but for this model, we computed similarity based on the tags each user has ever used. Again, we tuned the threshold based on validation set performance, and ofund 0.06 to be the best.

### Model 6, acc ~ 75%
Same as **Model 1**, but checks if any of this user's friends *of friends* have listened to the artist.

### Model 7, acc ~ 68%
Same as **Model 2**, but for this model, instead of checking similarity with *all* the users that listened to this artist, we checked similarity with the intersection of that set and the user's friends.

### Model 8, acc ~ 76%
Same as **Model 4**, but additionally check if any of the user's friend's top tags are shared by the artist.

### Model 9, acc ~ 78%
Same as **Model 2**, but the similarity score for this model included both the artist similarity and the tag similarity between the given user and each of the users that listened to this artist. We tuned the Jaccard threshold based on validation set performance and found the best to be 0.05 again.

### Model 10, acc ~ 78%
Same as **Model 9**, but instead of using all the user's tags in the similarity calculation, we only used the top tags by frequency (like in **Model 3**). We got the best performance on the validation set when using the top 22 tags.

### Model 11, acc ~ 81%
Same as **Model 2**, but for the similarity calculation, instead of using all of the artists from each user, we used the artists with the top `n` listens for each user. We found that `n = 21` gave the best performance on the validation set.
