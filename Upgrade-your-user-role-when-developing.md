Right now we have 3 roles to use the site with.

When you make a user account, that is the initial role, we call that a `user` role. It cannot do anything other than see it's own profile. The profile location will be where we have a link to say "Click here to request to be an author" This will go to a form that has information and simply sends an email to Josh to review before making them an Author. This process will be refined as we get data and feedback.

For now, the user role cannot do anything. If someone makes an account, an administrator can see this user, and promote them to either the Author or the Administrator role.

From api version `0.9.0` onward we can select from the database to get the information we need to make ourselves a higher elevated user locally.

First, let's see the table describing the roles. For the remainder of this page, any of the SQL code (like below) will be run in PgAdmin in a new query window. 

`SELECT * FROM roles ORDER BY id;`

| id | role | description |
| ------ | ------ | ------ |
| 0 | user | Any public user that visits the site and makes an account. |
| 2 | author | Authors can add and modify words and words of the day. |
| 9 | admin | Administrators can add words and moderators |

Now, lets make sure we have the ap, and the website running locally.

You can make an account like normal.

Afterwards, return to the PgAdmin window and you can now see your user in the user table:

`SELECT * FROM users;`

| id | email | password | tokenVersion | passwordResetToken | passwordResetExpires |
| ------ | ------ | ------ | ------ | ------ | ------ |
| 1 | "mark@80px.com" | "$2afdsasdf987fsd$uWWSk55bv0fjd87sdf/dsf9873k2rjh93j9kj4bnw34" | 0 | null | null |

Now we can see that our user has an ID of 1. We can use this, and the ID's from the role table to insert a new role for this user.

Next, we can see a list of the languages we support, and their ID's in the languages table.

`SELECT * FROM languages;`

| id | language              | languageCode |
|----|-----------------------|--------------|
| 0  | English               | en           |
| 1  | Spanish               | es           |
| 2  | German                | de           |
| 3  | French                | fr           |
| 4  | Italian               | it           |
| 5  | Chinese (Simplified)  | zh_CN        |
| 6  | Chinese (Traditional) | zh_TW        |
| 7  | Russian               | ru           |
| 8  | Hindi                 | hi           |

Now we have all the information we need to update ourselves to a higher level user, either Author or Administrator.

Let's run this command to insert a new role into the profile table. The profile table has a list of possible roles associated with a language. (If we have an admin role inserted, we can assume we have access to all languages, so only one insert on any language is needed.)

Examples

Add yourself as an Author in English:

`INSERT INTO profile ("createdDate", "usersId", "roleId", "languageId", "assignedById") VALUES (NOW(), 1, 2, 0, 1);`

Or, add yourself as an Administrator:

`INSERT INTO profile ("createdDate", "usersId", "roleId", "languageId", "assignedById") VALUES (NOW(), 1, 9, 0, 1);`

Now we can see our role in the profile table.

`SELECT * FROM profile;`

| id | createdDate                | usersId | roleId | languageId | assignedById |
|----|----------------------------|---------|--------|------------|--------------|
| 1  | 2020-07-25 04:41:53.825515 | 1       | 2      | 0          | 1            |


Thats it! After you insert into the profile table, all you should have to do is refresh the page to see the effects. If you're an author, you'll see an author button in the profile menu dropdown, and an admin button if you're an admin.

