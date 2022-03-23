# 77075917086

[Wheeldle](https://wheeldle.us) is a popular variant of Wordle among middle/high school kids.   
Its both easier to play & more difficult at the same time!    
But don't take our word for it, [give it a spin! ](https://wheeldle.us)

Today's question comes to us from Arjun in 5th grade:   
Q. My friend says there are 2000 Wordle answers. So I can play 2000 different games of Wordle. How many Wheeldle games can I play ?

A. Well, Arjun, that's a pretty big number to compute, but we've got R on our side.     
So, lets begin.    
Wheeldle looks like this:

![Wheeldle!](https://avatars.githubusercontent.com/u/102187509?s=400&u=3837a0ec18ffd22b23d54e562d64c7bb9ab8c84b&v=4 "Wheeldle")


So we have 3 words that end in a common letter.   
And two words that start with the same common letter.


1. Superbad Estimate (upper bound): We have 21 characters in the wheel. There are 26 letters in the alphabet. So 21^26.

    > 21^26    
    2.386172e+34

That's 2 followed by 34 zeros! A very big number indeed. But its definitely the wrong answer. You can't just pick any letter of the alphabet for each of the 21 positions....you'd get nonsense words like AAAAA. 

2. Less Bad Estimate: Wheeldle & Wordle use the same dictionary. You could choose 3 words for the top half, and then 2 words for the bottom. So choose 5 words out of the 2000 words ?

    > choose(2000,5)    
    2.653357e+14

That's 2 followed by 14 zeros! Smaller than before, but still quite big. Again, wrong answer. You can't just pick any 5 words! The first three words must end in the same letter of the alphabet the last two words begin with. What are the chances of that ? 

3. Better Estimate: Well, what ARE the chances that the first three words end in the same letter of the alphabet the last two words begin with ? Offhand, I'd say, oh, 1 in a million ? So that gives us:

    > choose(2000,5)*1e-6    
    265335665
    
4. Exact Solution: Lets first gather all the 2000+ answers allowed by Wordle in an array:

    > words <-readLines("wordle.txt" , n = -1)  # The 2000 answers are sitting in a text file wordle.txt

We want to find the first character of each word:

    > substr(words,1,1)
    
and the last character of each word:

    > substr(words,5,5)
    
then match these to the letters (helpfully called letters in R! ) in the alphabet:

    > charmatch(substr(words,1,1), letters)
    
and run a frequency count for each letter:

    > startsWith <- tabulate(charmatch(substr(words,1,1), letters))
    > endsWith <- tabulate(charmatch(substr(words,5,5), letters))
    
and multiply those frequency counts, choosing 3 if it ends in a letter, or choosing 2 if it starts with the same letter:

    > choose(endsWith,3) * choose(startsWith,2)
    
 and add up those 26 products:
 
    > sum(choose(endsWith,3) * choose(startsWith,2))
   
 And we're done! Guess what that number is ?
 
    > words <-readLines("wordle.txt" , n = -1)
    > startsWith <- tabulate(charmatch(substr(words,1,1), letters))
    > endsWith <- tabulate(charmatch(substr(words,5,5), letters))
    > numgames<-sum(choose(endsWith,3) * choose(startsWith,2))
    > print(numgames)
    77075917086
    
 That's enough math for the day, lets play [Wheeldle!](https://wheeldle.us)
