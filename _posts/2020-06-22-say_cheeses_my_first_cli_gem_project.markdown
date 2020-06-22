---
layout: post
title:      "SAY CHEESES! MY FIRST CLI GEM PROJECT"
date:       2020-06-22 09:28:14 -0400
permalink:  say_cheeses_my_first_cli_gem_project
---


I have finished the majority of my CLI project! I want to take a pause here and reflect on my coding journey so far. To be honest,  I didn't expect this project would be such a challenge at first. The project requirements seem to be pretty simple and straight forward.  All I need to do is to implement what I have learned in the past month, easy peasy. But boy, was I wrong. When I sit in front of my laptop and ready to code, I went [🤯 <=mind blowing] for about 2 days. OMG?! Where should I start? There are so many readme links and resources  (They are helpful, but it's a LOT). There is no RSpec code to guide me through? 
    
 As a new developer, the starting off of the project was the hardest part hands down. Decide between what I want to do and what I'm capable of doing is a struggle. I want to do something useful to me or something I'm interested in. After testing out a few ideas and figured out some of the websites I wanted to build on are not scrapable.  I finally settled down with cheese. Who doesn't like cheese!? 

 I decided to build a gem over the website https://www.castellocheese.com/en-us/cheese-types/ . Cheese lovers can explore cheeses by their types, and within each type, they can learn what are the classic cheeses under the category, and they can further learn more about each cheese's characteristics and waht is the best wine to pair with. 
    
Alright, I have to admit, Ultra-long tech blogs always lost me in the first few paragraphs, so I would like to keep mine short and sweet. Let's get into it. 

**First, I started to write down the workflow from the user's perspective.** 
I would not be explaining too much here. I think the below chart explains it all. Of course, this is only the final version, I started only a portion of the chart and edited it along the way

![](https://i.imgur.com/JkO79EU.jpg)

**Then, I built the structure from the developer's point of view. 
**

![](https://i.imgur.com/9Botwor.jpg)

After I was more or less clear of what I wanted to do. The coding part was actually the easy part of the project. It did not take me too long to get the apps working. 

**After the apps works. I started to clean up and refactored my code.**

I extracted some functionalities like #print_list, #valid_input? to make the code more readable. 

**The last part is the fun part. I played around with colorize and added a fun function  like #box_a_string to make the presentation prettier and enhance user experience.**

```
def box_a_string(string)
    puts ""
    print " "
    (string.length+2).times do print "_" end
    puts ""
    puts ""
    puts "| #{string} |"
    print " "
    (string.length+2).times do print "_" end
    puts ""
  end

```

![img](https://i.imgur.com/vImrTwF.jpg)

**An unexpected set back at the end of the projects.**
Although, I checked the content of the website was scrapable at the beginning of the project. When I scraped the cheese_type information page toward the end of the project. I realized there is no special CSS selector to separate cheese_description and wine_pair attribute, and they all are all <p> elements.


![img](https://i.imgur.com/gMcBf3w.jpg[/img])


For the sake of extracting the information that I wanted with the knowledge I have. I need to do something wanky. After analysis of the content. I realized that after grabbing all <p> elements. From the last, every 2 paragraphs correspond with one cheese. The first one is the characteristics, the second one is the wine pair. So I use my knowledge of array and extracted the data that I wanted correctly. I know that it is not good real-world practice, but it's a demonstration of my problem-solving ability, and I'm happy that I saved myself from redo this project.

```
  def self.scrape_cheeses(cheese_type)
    @@cheese_type = cheese_type
    url = BASEPATH + @@cheese_type.name.gsub(" ","-") + "/"
    doc = Nokogiri::HTML(open(url))
    heads = doc.css("div.cheese-category__inner.row-container h4")
    cheese_type.char = heads.shift.text
    heads.each.with_index do |h|
      name = h.text
      CheeseBoard::Cheese.new(name, cheese_type)
    end

    description_count = @@cheese_type.cheeses.count*2
    descriptions = doc.css("div.cheese-category__inner.row-container p")
    t = descriptions.map {|x| x.text}.last(description_count)
    c_d = t.select.with_index(1) {|_,i| i.odd?}
    p_w = t.select.with_index(1) {|_,i| i.even?}
    @@cheese_type.cheeses.zip(c_d , p_w).each do |cheese,c_des,p_des|
      cheese.cheese_description = c_des
      cheese.pair_wine = p_des
    end
  end

```

I learned a lot from doing this project and went through the whole development cycle. I feel I gain more clarity on what I've learned so far. Knowledge comes from practice. I planned to do more gem projects in my free time to practice. I believe until things start to come naturally to you, you are not getting it.  There also is room to improve and build on this gem. I cannot wait to embrace what comes next to my way.




