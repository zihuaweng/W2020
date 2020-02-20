# PacMan Assignment

## 1. What is the role of EmptySprite?

```EmptySprite.java``` is the implementation of the interface `Sprite.java`. It creates an empty sprite and draws nothing. This is called in two other classes, `AnimatedSprite.java` and `ImageSprite.java`. 

-  In `ImageSprite.java`, when the sprite is out of boundary, it will return an empty sprite.

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

- In `AnimatedSprite.java`, when current frame reaches the end of whole frame, it will return an empty sprite.

  ```java
       private static final Sprite END_OF_LOOP = new EmptySprite();
       /**
       * @return The frame of the current index.
       */
      private Sprite currentSprite() {
          Sprite result = END_OF_LOOP;
          if (current < animationFrames.length) {
              result = animationFrames[current];
          }
          assert result != null;
          return result;
      }
  ```



## 2: What is the role of MOVE_INTERVAL and INTERVAL_VARIATION?

These two values are defined in the super class `Ghost.java`.

```java
     /**
     * The time that should be taken between moves.
     * @return The suggested delay between moves in milliseconds.
     */
    public long getInterval() {
        return this.moveInterval + new Random().nextInt(this.intervalVariation);
    }
```



- MOVE_INTERVAL:

  The speed of sprite is controlled by this value. 

- INTERVAL_VARIATION

  This adds more randomness to the different ghosts.

  

## 3. If you wanted to add a fruit, which files would you need to change?

1. Update the `resources/board.txt` by inserting 'M' in it.

   ```
   ###############M#######    // add mango here
   #..........#..........#
   #.###.####.#.####.###.#
   #.....................#
   #.###.#.##M####.#.###.#    // And here
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
   #.....................#
   #######################
   ```

2. Give Mango a picture and create the mango in `PacManSprites.java`.

   ```
   public Sprite getMangoSprite() {
       return loadSprite("/sprite/mango.png");
   }
   ```

   

3. Define the point when Pacman eats the mango and create the mango in `LevelFactory.java`.

   ```java
   private static final int MANGO_VALUE = 30;
   ```

   ```
   ![result](C:\Users\dlwan\Documents\Study\reverse_engineering\result.jpg)public Pellet createMangoPellet() {
           return new Pellet(MANGO_VALUE, sprites.getMangoSprite());
       }
   ```

   

4. Add the mango condition in `MapParser.java`, to deal with the 'M'.

   ```java
   case 'M':
       Square mangoSquare = boardCreator.createGround();
       grid[x][y] = mangoSquare;
       levelCreator.createMangoPellet().occupy(mangoSquare);
       break;
   ```

   Result is shown below.

   ![Results.png](https://i.imgur.com/lSfZ9IB.png)

