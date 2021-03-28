---
layout: post
title:      "Some Tips on deploying your project to Heroku!"
date:       2021-03-28 13:33:18 -0400
permalink:  some_tips_on_deploying_your_project_to_heroku
---


I deployed 3 of my flatiron projects last week and had my share of frustration along the way. It turns out to be a bigger hustle than I expected. Everything works until it doesn't. I just want to share some of my tips and experience.

Preparation Before Deploying:
1. Rails API should use SQLite as a database by default, if you didn't specify it by running rails new your-app-name-here --api --database=postgresql`when you created the project. You are using the defaulted database Sqlite. You will need to switch if from SQLite to PostgreSQL to be able to deploy your project on Heroku. The procedure is fairly simple as long as you download Postgres and serve it correctly. You can refer this [article](https://medium.com/@virtual_khan/converting-rails-from-sqlite3-to-postgresql-d97023314a14).

Note: 
a. PostgreSQL is available in all Ubuntu versions by default, if you have Ubuntu, you probably should install via Ubuntu. 
b. Postgres has to be online. If you got the error "error: could not connect to database Postgres: could not connect to server: No such file or directory Is the server running locally and accepting connections on Unix domain socket "/var/run/postgresql/.s.PGSQL.5432"?
Try `sudo service postgresql start` to start postgres, and then `sudo service postgresql status` to check the status

2. You should create a Heroku account.

3. You should install Heroku CLI, please refer[ this](https://devcenter.heroku.com/articles/heroku-cli#getting-started)

Depends on your file structure, the procedure is slightly different.

Unless your app is a complete full-stack rails app, and you have a frontend and backend, you will treat them as two stand-alone applications to deploy.

**File Structure
**

For my js/rails app, both frontend and backend are in the same repo. 

* backend/QTee
* frontend

**First let's deploy the backend to Heroku**
 in the root directory of the application, I used following commands:

Step 1: Log into Heroku from the command line, and follow the prompts:
`heroku login`

Step 2: Initiate the new repository for Heroku deployment:
`git init`

Step 3: Create and name the Heroku repository. I recommend tagging "-api" to the name so you know it is an api.
`heroku create NAME_OF_APP_API`

Step 4: Add and commit your changes:
```
git add .
git commit -m "first commit"
```

Step 5: Push your commit to Heroku master. This will be done in different ways depending on your file structure:

If your Rails API is in its standalone repository:
`git push heroku master` 
*If your branch name is main, use` git push heroku main` instead

If your Rails API is in a folder within the root directory (as mine is):
`git push --force heroku 'git subtree split --prefix NAME_OF_BACKEND_FOLDER HEAD':master`
In my case the command should be:
`git push --force heroku 'git subtree split --prefix backend/QTee':master`

Note: This command splits the Rails API backend directory from the rest of the repository, and pushes just that sub-tree to Heroku for deployment.

Step 6: Resetting the Postgres Database
heroku rake db:schema:load
heroku rake db:migrate
heroku rake db:seed

Note: When I seeded my db, I ran into errors: PostgreSQL: Unique violation: 7 ERROR: duplicate key value violates unique constraint “users_pkey”. After some digging, I found out that Postgres handles auto-incrementing a little differently than Sqlite does. In Postgres, when you create the serial field, you are also creating a sequence field that is keeping track of the id to use. This sequence field is going to start with a value of 1. 

When you insert a new record into the table, if you don't specify the id field, it will use the value of the sequence, and then increment the sequence. However, if you do specify the id field, then the sequence is not used, and it is not updated, either. So, if when you moved over to Postgres, you seeded or imported some existing users, along with their existing ids. When you created these user records with their ids, the sequence was not used, and therefore it was never updated. So, if, for example, you imported 10 users, you have users with ids 1-10, but your sequence is still at 1. When you attempt to create a new user without specifying the id, it pulls the value from the sequence (1), and you get a unique violation because you already have a user with id 1.

To solve this problem, you will have to set your users_id_seq sequence value to the MAX(id) of your existing users. 
Ran` heroku run rails c`
and then run the below code in the console
```
ActiveRecord::Base.connection.tables.each do |t|
  ActiveRecord::Base.connection.reset_pk_sequence!(t)
end
```

Step 7: You can now check your backend endpoints to check your data, make sure they are correctly served.
* If you don't have a homepage set up, it will run into an error like "page is not found". It makes sense right, there is no data to display on the homepage, you can either ignore it and navigate to the routes you already defined that serve data, or set a dummy root route in config/routes.rb. 

Now that backend is all set. Let's move on to the frontend.

**You can deploy frontend to Heroku, gitpage (I found it much easier) or Netlify. **

Before deployment. You will need to update your fetch request URL to newly deployed Heroku backend URLs.
If you chose to deploy frontend to Heroku as well. You will do it by repeat the above procedure. It would be the same as when you deploy the backend. For Heroku, they are just applications. Of course, you will emit the database setup part.

If you choose to deploy it to GitHub, The process is described in their [documentation](https://pages.github.com/), and you can skip ahead to their Step 4 since you already have your app created.

Step 1: Click on the Settings tab and scroll to GitHub Pages (at the bottom).

Step 2:Choose a branch that you want to deploy (most likely Master).

Step 3: Navigate to the URL following this pattern:
http://username.github.io/repository

* If your frontend and backend are saved in the same repository, you will need to change the URL accordingly to access your index.html site. 
* for example, if my file structure is
  * backend/QTee
  * frontend

Important Note: If your frontend and backend are saved in the same repository, you will need to change the URL accordingly to access your index.html site.
The actual URL that I can find on my page would be http://username.github.io/repository/frontend

Some other tips:
1. If you have env variable set up in your backend, such as your Bcypt secret, your omniauth client secret, and client id, you will need to set it up with Heroku as well. 
Otherwise, your auto-login or your omniauth login won't work. 
You can set up heroku env variable follow their [documentation ](https://devcenter.heroku.com/articles/config-vars)

2. If you find out your fontend doesn't render correctly, and you are using react-bootstrap <Col> <Row>, that might be the reason. Some bootstrap style just doesn't render well. Don't ask me why, I went crazy over this for a week to find out why my front end doesn't render correctly. When I check my element tab, the node is there, attached correctly, it just doesn't 'show' on the screen. I fixed it by writing plain CSS as well.
Don't panic when your site works locally, but behave weird after deploying to Heroku. Find the possible reasons and eliminate them one by one by writing substitute code and drill down to the real problem.
