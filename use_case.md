### A short discription of the use case diagram.


 ![alt text][use_case]

- Actors
  - There is only one actor, which is a site user, who is a user on the site, who will usually be refered to as Bob.
- Use cases
  - As a user you can Browse Stories posted on the site.
  - As a user you can register, so the system can recognize you.
  - As a registered user you can login, so that others can recognize you, and gain access to other features on the site.
  - As a logged in user you can post stories and/or comments, so that other users may read them.
  - As a logged in user you can upvote stories and comments and gain 1 karma per upvote. 
  - As a logged in user you can flag any content for spam.
  - As a logged in user with more than 500 karma you can downvote stories and comments.
  - As a user you can try out our own Loanbroker system from the System Integration Course.

| **Use Case Name** | User upvotes and downvotes |
| :--- | :--- |
| **Participating actor instances** | **Bob** :Logged in User |
| **Flow of events** |   1. While Bob is logged he can browse all stories and comments. <br>   2. Bob finds something he likes and clicks upvote <br>   2. The creator of said story or comment then gains one karma <br> 3. Bob then browse other stories and finds some offensive content and clicks the downvote feature<br>   4. If Bob has more than 500 karma then the site accepts the downvote, if not nothing happens.|


[use_case]: https://github.com/ElDuderino420/HackerNewsClone/blob/master/use_case_diagram.png "A complete use case diagram"