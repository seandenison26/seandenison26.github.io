---
layout: post
title:  CLI Project: Is This Really the Right Pattern?
date:   2017-05-31 01:28:13 +0000
---



> "Beware of bugs in the above code; I have only proved it correct, not tried it."
- Donald E. Knuth.

So after a weekend of equal amounts of filthy language and giddy epiphanies I am just about ready to submit my CLI project. I used to spend more hours on video games than anyone with a fulltime job ever should but my current endeavor to understand the language and thinking of our eventual computational overlords has left me with little time to devote to blowing up things. This meant that the logical choice for my chosen project was to make it easier to play a game that I no time to play anymore, Heroes of the Storm. 

If you've ever had the misfortune to play a MOBA like HOTS, Leagues of Legends, SMITE, or that shamelssly blatant ripoff of the genre, Dota 2, you'll be very famialiar with the sensation I like to refer to as "too much damn crap to know" syndrome. Each of these games has nth amount of heroes with nth amount of abilities and you'll quickly be reprimanded in your first couple of games for not knowing you should never jungle with Riki unless Sonya is in the top and Roshan is in his lactose-intolerant phase. To make this experience maginally easier, and that margin would be "just-barely-qualifying-the-spec.md", I wrote a CLI that would scrape the top build from www.heroesfire.com and display the information. I don't expect this to make me a better player but at leat I'll hopefully spend less time searching for a way to build my hero that doesn't result in my team considering all the better things they could be doing with their life.

# CLI
So while I'm sure you'd all love to see the long strings of function chain I had to sling together to any get anthing useful from the table formatted data (the regex, oh god, the regex....) the part of my program my title is referring to is my CLI, shamefully shown here:

```
def call
	welcome
	view = gets.chomp
	puts "\n"
	while view != "exit" do
		case view
			when Hero
				hero = view.dup.freeze
				puts hero_data(view)
				puts build_list(view)
				puts "Enter the number of a build to see that build or type \"hero search\" to search for a new hero"
				view = gets.chomp
				if view.to_i > 0 && view.to_i <= hero.builds.size
					view = hero.builds[view.to_i - 1]
				end	
			when Build
				build = view.dup.freeze
				puts build_levels(view)
				puts "If you would like to go back and see other hero builds type \"go back\". To search another hero type \"hero search\"."
				view = gets.chomp
				if view == "go back"
					view = build.hero.name
				end
			when "help"
				help
				view = gets.chomp
			when "free"
				puts free_list
				view = "hero search"		
			when "hero search"
				puts "What Hero would you like to see?"
				view = gets.chomp
			when String
				view = Hero.find_or_create_by_name(view)
			else
				puts "I'm sorry, that made no sense to me."
				view = gets.chomp
		end
		puts "\n"	
	end
end
```

So while I'd love to say that I was misguidedly given this pattern in some strange corner of the internet by a man in a trench coat and bare shins who told, "Oh don't worry, everyone's using it! What are you waiting for? It'll look great, I promise...," unfortunatly this is all my own design. I knew I wanted  the user to be able to pass certain commands like "go back" and "exit" but I also wanted the ability to pass a hero name and just get a hero. The giggle that emitted my person when I realized a case statement would evaluate an Object type to true like the way I'm using it here should never be heard when you know the person who made it looks like he came from an episode of Vikings. My one concern with the pattern is the use of the `hero = view.dup.freeze` in the first two casses but I didn't want to default back to the else message of the case statement any time the user mistyped or couldn't spell.

All in all I enjoyed the project. Managed to devekop the entire thing on my personal computer with Vim, and if any one you use windows you know how much of an adventure dev can be. Hope you were entertained. I'm gonna back to screwing up SQL statements and wishing the Vim package for Atom allowed for colon statements.

Yours In State,

Ziggy


