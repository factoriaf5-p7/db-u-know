### 1 user can sign up into the system
```sql
INSERT INTO user values(
    name,
    email,
    password
)
```
### 1 register user (RU) can log in the system
```sql
SELECT email, password FROM user WHERE email = <EMAIL_ADDRESS>
```

### Any user can visualize a rating content list
```sql
SELECT id, title, description, header_image FROM content ORDER BY rating limit 10
--if the user has already been logged in then we must cross this query with the user_content table to check the list content as purchased.
```
### 1 RU can select one content from list

```sql
-- Alternative 1 --> the content has already been purchased by the user
SELECT * FROM content where id = <CONTENT_ID>
-- Alternative 2 --> the content is new to the user
SELECT price FROM content where id = <CONTENT_ID>
SELECT balance FROM user where id =<USER_ID>
```
```js
//Coding section
if (balance >= price){
    balanceVariable -=price
```
```sql
    UPDATE user SET balance = balanceVariable WHERE id = <USER_ID>
    INSERT into user_content values(
        <USER_ID>,
        <CONTENT_ID>,
    )
    SELECT * FROM content where id = <CONTENT_ID>
```
```js
} else {
    //message to the USER >> NO BALANCE AVAILABLE. YOU SHOULD CREATE CONTENT TO PURCHASE CONTENT
}
```

### 1 RU can create content
First there is an AI approval test as a middleware.
```sql
INSERT INTO content values(
    title,
    description,
    header_image,
    content
    author_id
)
```

### 1 RU can edit content
```sql
UPDATE content set <PROPERTIES_TO_EDIT> where id = <CONTENT_ID> and author_id = <USER_ID>
```

### 1 RU can remove content
```sql
SELECT content_id from user_content
```
```js
//Coding section
if(!content_id){
    //message to the USER >> your balance will be affected by this decision. Do you want to continue?
```
```sql
    SELECT price from content where id =<CONTENT_ID>
    SELECT balance from user where id = <USER_ID>
    DELETE FROM content where id = <CONTENT_ID> and author_id = <USER_ID>
```
```js
//coding section
   balanceVariable = balance-price
```
```sql
    UPDATE user SET balance = <balanceVariable> WHERE id = <USER_ID>
```
```js
} else{
    //message to the USER >> YOU CAN NOT REMOVE CONTENT THAT HAS ALREADY BEEN PURCHASED.
}
```

### 1 RU can rate a purchased content
```js
//check RATING_VALUE <= 5
```
```sql
UPDATE user_content set user_rating=<RATING_VALUE> where user_id =<USER_ID> AND content_id=<CONTENT_ID>
--update the content average rating
SELECT content_id, user_rating FROM user_content
```
```js
//code section --> calculate rating content
if(rates.length >= 5){
    const avg = rates.reduce((ac, e,i,a)=> ac += e.user_rating/a.length)
```
```sql
    UPDATE content SET rating = avg
```
```js
    if(rating <= 3)
        contentPrice -= 20%
```
```sql
        UPDATE content SET price = contentPrice
```
```js
} else {
    rates.map(e => e.user_rating = 4.8 ).reduce((ac, e,i,a)=> ac += e.user_rating/a.length)
}
```


