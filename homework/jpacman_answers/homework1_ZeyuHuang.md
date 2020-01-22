## Assignment:
Continue to explore JPacMan3 by answering the following questions:
+ Q1: what is the role of EmptySprite?

  + EmptySprite means sprite with nothing in it literally, which can be found both in the implementations of the class. Both the width and the height are 0, and nothing is drawn when the method `draw()` is called.

    ```java
    /**
     * Empty Sprite which does not contain any data. When this sprite is drawn,
     * nothing happens.
     *
     * @author Jeroen Roosen 
     */
    public class EmptySprite implements Sprite {
    
        @Override
        public void draw(Graphics graphics, int x, int y, int width, int height) {
            // nothing to draw.
        }
    
        @Override
        public Sprite split(int x, int y, int width, int height) {
            return new EmptySprite();
        }
    
        @Override
        public int getWidth() { return 0; }
    
        @Override
        public int getHeight() { return 0; }
    
    }
    ```

  + The class is used when the pixel (x,y) is beyond a image sprite.

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

  + Or the loop is end. 

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

    

+ Q2: what is the role of MOVE_INTERVAL and INTERVAL_VARIATION?

  + These two variable appear in the implementation of different kinds of Ghost ( `Clyde`, `Blinky`, `Inky`, and `Pinky`). They are passed as parameters to the Constructor of Ghost, which means we can define different behaviors for different kinds of Ghost.

  + In the super class `Ghost`. We find that these two variable are saving as `moveInterval` and `intervalVariation`, which are used to calculate the moving interval of a ghost.

    ```java
    /**
         * The time that should be taken between moves.
         *
         * @return The suggested delay between moves in milliseconds.
         */
        public long getInterval() {
            return this.moveInterval + new Random().nextInt(this.intervalVariation);
        }
    
    ```

  + `moveInterval` defines the basic moving interval, and `intervalVariation ` is used to make the ghosts look more dynamic and less predictable.

+ Q3: if you wanted to add a fruit, which files would you need to change?

  + Fruit can be defined as a super pellet with bonus points and different icons.

  + Define the score earned when the Pacman eats a fruit in **LevelFactory.java**.

    ```java
    private static final int FRUIT_VALUE = 20;
    ```

  + Add the `getFruitSprite(String fruit)` function in **PacManSprites.java**

    ```java
    public Sprite getFruitSprite(String fruit) {
            return loadSprite("/sprite/" + fruit + ".png");
        }
    ```

  + Add the `createFruit(String fruit)` function in **LevelFactory.java**

    ```java
    public Pellet createFruit(String fruit) {
            return new Pellet(FRUIT_VALUE, sprites.getFruitSprite(fruit));
        }
    ```

  + Add cases for different fruit in the `addSquare()`function in **MapParser.java**.

    ```java
    case 'A':
    case 'C':
    case 'M':
    case 'O':
    case 'S':
    	Square fruitSquare = boardCreator.createGround();
    	grid[x][y] = fruitSquare;
    	levelCreator.createFruit(FRUIT_MAP.get(Character.toString(c))).occupy(fruitSquare);
    ```

  + Edit the Map in **board.txt**

    ```txt
    #######################
    #..MM......#......A...#
    #.###.####.#.####.###.#
    #......A....M.........#
    #.###.#.#######.#.###.#
    #..S..#....#.M..#..S..#
    #####.#### # ####.#####
        #.#    G    #.#    
    #####.# ##   ## #.#####
         .  #G G G#  .     
    #####.# ####### #.#####
        #.#         #.#    
    #####.#  #####  #.#####
    #..........#...O......#
    #.###.####.#.####.###.#
    #...#..M...P......#...#
    ###.#.#.#######.#.#.###
    #.....#....#....#.....#
    #.########.#.########.#
    #.............C.......#
    #######################
    ```

  + Here comes the fruits. We earn 50 scores by eating 3 pellets and 1 melon, which performs as we wish.

    ![image-20200119002220478.png](https://i.loli.net/2020/01/21/xwDh3U27qIGoNSe.png)

    ![image-20200119001832351.png](https://i.loli.net/2020/01/21/pEhHLtlaqd9Krxy.png)

