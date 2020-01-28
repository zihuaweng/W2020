# Pacman3 assignment

**Q1: What is the role of EmptySprite?**

+ There are three implementation classes of the interface ***Sprite.class***, and ***EmptySprite.class*** is used to implemented an empty sprite, which draws nothing and has both width and length of zero.
  + ImageSprite
  + AnimatedSprite
  + EmptySprite

+ In the ***ImageSprite.class***, the function ```split``` is used to split a static image into frames. When the coordinate is out of boundary, we will return a instance of EmptySprite to draw nothing, which makes the sprite disappear.

```java
@Override
public Sprite split(int x, int y, int width, int height) {
		if (withinImage(x, y) && withinImage(x + width - 1, y + height - 1)) {
    		BufferedImage newImage = newImage(width, height);
        newImage.createGraphics().drawImage(image, 0, 0, width, height, x,
        	y, x + width, y + height, null);
      	return new ImageSprite(newImage);
    }
    return new EmptySprite();
}
```



+ In `AnimatedSprite.class`, it will constantly call the function ``` draw``` to update the position of sprite. When the pacman dies, the variable *current*  should be 11, which is out of boundary. So it will return an instance of EmptySprite. When we call the function ```draw``` in the EmptySprite, it will draw nothing, which make the sprite disappear.

```java
private Sprite currentSprite() {
    Sprite result = END_OF_LOOP;
    if (current < animationFrames.length) {
      result = animationFrames[current];
    }
    assert result != null;
    return result;
}
```



---



**Q2: What is the role of MOVE_INTERVAL and INTERVAL_VARIATION?**

The MOVE_INTERVAL and INTERVAL_VARIATION are both used to set the delay between the move of ghost in milliseconds. The corresponding proceed is as below.

```java
public long getInterval() {
    return this.moveInterval + new Random().nextInt(this.intervalVariation);
}	
```

> This code uses MOVE_INTERVAL and INTERVAL_VARIATION to get a random value between [MOVE_INTERVAL, MOVE_INTERVAL + INTERVAL_VARIATION) to make the interval of ghost's move randomly.



---



**Q3: If you wanted to add a fruit, which files would you need to change?**

1. Add a new char 'A'  as below(as the comment), to represent an apple

   ```
   #######################
   #..........#..........#
   #.###.####.#.####.###.#
   #.....................#
   #.###.#.#######.#.###.#
   #.....#....#....#.....#
   #####.#### # ####.#####
       #.#    G    #.#    
   #####.# ##   ## #.#####
        .  #G G G#  .     
   #####.# ####### #.#####
       #.#         #.#    
   #####.#  #####  #.#####
   #..........#..........#
   #.###.####.#.####.###.#
   #...#......P......#...#
   ###.#.#.#######.#.#.###
   #.....#....#....#.....#
   #.########.#.########.#
   #..................A..#          //add an 'A' here
   #######################
   ```

   

2. Parse 'A' from the board.txt to add an apple, as the handling in the ```case 'A'```

   ```java
   protected void addSquare(Square[][] grid, List<Ghost> ghosts,
                                List<Square> startPositions, int x, int y, char c) {
           switch (c) {
               case ' ':
                   grid[x][y] = boardCreator.createGround();
                   break;
               case '#':
                   grid[x][y] = boardCreator.createWall();
                   break;
               case '.':
                   Square pelletSquare = boardCreator.createGround();
                   grid[x][y] = pelletSquare;
                   levelCreator.createPellet().occupy(pelletSquare);
                   break;
               //parse apple from char 'A'
               case 'A':
                   Square appleSquare = boardCreator.createGround();
                   grid[x][y] = appleSquare;
                   levelCreator.createApplePellet().occupy(appleSquare);
                   break;
               case 'G':
                   Square ghostSquare = makeGhostSquare(ghosts, levelCreator.createGhost());
                   grid[x][y] = ghostSquare;
                   break;
               case 'P':
                   Square playerSquare = boardCreator.createGround();
                   grid[x][y] = playerSquare;
                   startPositions.add(playerSquare);
                   break;
               default:
                   throw new PacmanConfigurationException("Invalid character at "
                       + x + "," + y + ": " + c);
           }
       }
   ```

   

3. Add funtions to create an apple image and set its points

   ```java
   public Pellet createApplePellet() {
   		return new Pellet(APPLE_VALUE, sprites.getApplePelletSprite());
   }
   ```

   ```java
   public Sprite getApplePelletSprite() {
       return loadSprite("/sprite/apple.png");
   }
   ```

   ```java
   private static final int APPLE_VALUE = 50;
   ```

   

