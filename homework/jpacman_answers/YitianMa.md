# Continue to explore JPacman:

## Assignment:
Continue to explore JPacMan3 by answering the following questions:
+ what is the role of EmptySprite?
	-   EmptySprite is a Sprite that does not contain any data(image).
	-   EmptySprite takes care of null pointers. When the program encounters a null pointer, it can still execute smoothly and call methods such as draw(), split() like other normal sprite does without being interrupted by unexpected edge cases.
	-   EmptySprite can also be pretty useful if we want to draw some invisible objects like fruits for bonus points.
	-   Blocks of code I found helpful to answer this question
	[![Screen-Shot-2020-01-17-at-11-14-52-PM.png](https://i.postimg.cc/NjHWG8pM/Screen-Shot-2020-01-17-at-11-14-52-PM.png)](https://postimg.cc/bZq63nXc)
[![Screen-Shot-2020-01-17-at-11-15-20-PM.png](https://i.postimg.cc/44KCFTVV/Screen-Shot-2020-01-17-at-11-15-20-PM.png)](https://postimg.cc/NLwP5SNj)

+ what is the role of MOVE_INTERVAL and INTERVAL_VARIATION?

	- INTERVAL_VARIATION is the variation in intervals. All ghosts have the same value of INTERVAL_VARIATION, which equals to 50.  
	- MOVE_INTERVAL is the base move interval of ghosts.
	- INTERVAL_VARIATION and MOVE_INTERVAL together suggest the time delayed between moves in milliseconds for each ghost. It seems that the ghost “Pinky” a.k.a. “Speedy” has the quickest move because its MOVE_INTERVAL is the smallest.
	- Blocks of code I found helpful to answer this question:
	[![Screen-Shot-2020-01-17-at-11-32-15-PM.png](https://i.postimg.cc/N0PJ15NV/Screen-Shot-2020-01-17-at-11-32-15-PM.png)](https://postimg.cc/4KVQGNRb)
	[![Screen-Shot-2020-01-17-at-11-32-40-PM.png](https://i.postimg.cc/s2tPjSHV/Screen-Shot-2020-01-17-at-11-32-40-PM.png)](https://postimg.cc/FfVkGfQq)
	[![Screen-Shot-2020-01-17-at-11-34-28-PM.png](https://i.postimg.cc/y8x00nw1/Screen-Shot-2020-01-17-at-11-34-28-PM.png)](https://postimg.cc/pmN9HQW4)
	[![Screen-Shot-2020-01-17-at-11-34-41-PM.png](https://i.postimg.cc/G3jDNZR3/Screen-Shot-2020-01-17-at-11-34-41-PM.png)](https://postimg.cc/s1v1GH1b)
	

+ if you wanted to add a fruit, which files would you need to change?
	
	Adding a fruit is not that different from adding a pellet, with such assumptions, I figured out how pellets are created and displayed in this game, and applied the same pattern for fruits. To hide fruites underneath pellets, we can return EmptySprite when calling getSprites() in the Fruit class. Here is a possible solution I came up with for this question: 
	1. **board.txt** - Wherever we want to add fruits, change the character '.' to another special character that has not been used in the board to specify the coexistence of a pellet and a fruit.  
	2. In module level, create a new class called **Fruit**.
	3. In module level - **LevelFactory**, add a new method called CreateFruit(), which involves the usage of getFruitSprite() specified below.
	4. In module level - **MapPaser**, inside addSquare(), add a new case for fruits: levelCreator.CreateFruit().occupy(fruitSquare);
	5. In module sprite - **PacManSprites**, add a new method called getFruitSprite().
	
## Submit:
- Create a pull request containing *only* your assignment.
- The assignment should be located in the *homework/jpacman_answers* directory
- Deadline: **Wednesday (January 22nd) at 3pm**
