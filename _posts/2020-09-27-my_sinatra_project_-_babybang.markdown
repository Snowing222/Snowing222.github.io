---
layout: post
title:      "My Sinatra Project - BabyBang"
date:       2020-09-27 08:56:40 -0400
permalink:  my_sinatra_project_-_babybang
---

 
     My Sinatra project is a baby meet up app for new parents just like me. I just had my son 8 months ago. Parenthood is something!! It probably is the hardest thing I have done in my life, it makes coding the 2nd hard one.  Once I became a parent, I felt I was automatically got recruited into a secret community. The ranking is given base on the hardship of the parenthood. How many babies do you have? How difficult these little machines are to operate? Yes, they came with some pre-setting. What is the operating situation? Do you get external support from family... When you strolling around the city and see other mom approaching you from far, with 2 toddlers running around and 1 infant crying in the stroller. You stop, give the right of the way, make eye contact sending positive energy, and pay respect.  This is how it works:)
	 
	 Joke aside, once I became a mama. I realized that boozy Sunday is not part of my life anymore. Instead, activities like strolling around the park, reading "adorable" books over and over again, listening to classical music with the baby hoping it will contribute to growing a little gentleman...start filling up my day. The activities that I cannot do with my "boozy Sunday friends" anymore. It would be nice to do those "baby things" with other parents in a similar situation or just do something for ourselves to keep our sanity while in a baby-friendly environment. However in my current friend's database. Another friend who just had a baby similar to my baby's age, and live in the same area. The chance is slim. I need new friends! That's why I want to build an app for parents who want to mingle and to plan activities for their little ones. Parents are in charge of babies' activities before babies can decide for themselves.
	 
	 I wanted to spend a week on the project. Starting from the basic requirement and see how far I can go with it. Below are the 6 phased of my progress.
	 
	 Phase One: To meet the basic requirements.  To have the authentication and core functionality working. I need 4 models: Parent, Baby, Playdate, Babyplaydate, and I need to wire them up correctly. 
        Parent should be able to CRUD playdate and their babies. 
![](https://i.imgur.com/KHqBsoC.jpg)
	
	 Babyplaydate is the join table between the baby and the playdate, holding two foreign keys baby_id and playdate_id. Parent as the user, handling all regarding authentication. I also set some validations at the database level, things like user name, user email have to be unique. It's all pretty straight forward. One thing worth mention is that originally I connected Parent with Playdate through the baby. 
	 
	 Theoretically, it makes sense. However, I ran into a problem when I want to query Parent's playdates. Since the baby is the join table. Parent can only query the playdates, and cannot create it. It can only be achieved by adding the playdate to parent's array when adding it to playdate's array. However, when a parent has more than one babies attend the playdate. It got duplicated data. To avoid this issue, I changed parent - playdate connection to a direct connection. Placed a parent_id in playdate as it's creator, so I can query parent's playdates directly without going through babies, and parent can create a playdate. It makes it more logical as well.
	 
	 Phase Two: After getting the basic functionality work. I decided to add the Comment feature. 
	 A commentor(parent) can have many commented_playdate(playdate) through comment
	 A playdate can have many commentors(parent) through comments.
	 I only need to add one new model Comment, and the rest is the name alias of the existing models - Parent and Playdate.
![](https://i.imgur.com/TI5PKla.jpg)

	 
	 
	 Phase Three: Add attend feature to playdates.
	 A parent can RSVP or un-RSVP a playdate as long as they are not the creator of the playdate.
	  Under the hood, it's just pushing or deleting the playdate to attended playdate array.
	 I create one more join table attend_playdate to join playdate and parent.
	 A playdate can have many attended_parent(parent) through attend_playdate
	 A parent can have many attended_playdate(playdate)through attend_playdate. Again, name alias is implemented in this case.
	 
	 So far, we alias parent model to commentor, and attended_parent, and playdate to commented_playdate and attended_playdate. To avoid confusion. I alias parent(who created the playdate) as the creator.
![](https://i.imgur.com/Qg90wIO.jpg)

	 
	 Phase Four: Refactor code.
	 Add some validations, and think about some edge cases, and how to handle bad data. Add flash message function.
	 
	 Phase Five: Refactor routes, and user experience.
	 Since I didn't have a blueprint of the app in the first place. I did not have a full picture in mind and I didn't decide how I want the interface to look like.  I need to do a lot of adjustments to enhance the user experience, to allow the user to have a good work flow using the apps. This is definitely what I should avoid in the future.
	 
	 The last phase: I played with BS and CSS. did a little bit of styling just for fun.
	 
	 There are more features  I want to add to this app. This project got me excited because it allows me to solve the real problem here. The more I'm into the program, the more knowledge I know, the more things I notice can be improved in life. It means the more problems I can potentially solve and make the world a better place. Well, those are pretty big words, let's start from a mimosa on Sunday morning with my new mama friends via BabyBang:)
