Soobin Choi\
1/19/2020\
SWE265P

# JPacman3 Homework

### 1. What is the role of the EmptySprite?

   The EmptySprite is used in the AnimatedSprite class and is instantiated at the end of a non-looping sprite, which is implemented through splitting the pacman.png into animated parts using the split class.

### 2. What is the role of MOVE_INTERVAL and INTERVAL_VARIATION?

It is apparent that both MOVE_INTERVAL and INTERVAL_VARIATION fields are used to animate ghost sprites; Pinky, Blinky, Clyde, and Inky.\
MOVE_INTERVAL is initialized as a private field for each ghost and the initialized value is the base movement interval for each ghost.\
INTERVAL_VARIATION is initialized as a private field for each ghost as well and the value represents the variation from the base MOVE_INTERVAL value for each ghost to make their movements more dynamic and unpredictable.\
The two values are used as parameters to create each ghost sprite in the game.\ 
```
private static final int INTERVAL_VARIATION = 50;

/**
 * The base movement interval.
 */
private static final int MOVE_INTERVAL = 200;

/**
 * Creates a new "Pinky", a.k.a. "Speedy".
 *
 * @param spriteMap
 *            The sprites for this ghost.
 */
public Pinky(Map<Direction, Sprite> spriteMap) {
    super(spriteMap, MOVE_INTERVAL, INTERVAL_VARIATION);
}
```

### 3. If you wanted to add a fruit, which files would you need to change? 

Essentially, fruits are fancier pellets. Therefore, in order to implement fruits into this game, I would first check for where and how pellets are implemented in the game source code, so that I can apply a similar logic in implementing fruits. 
The first occurrence of pellets can be found in DefaultPlayerInteractionMap.java, for the defaultCollisions() method. 
``` 
private static CollisionInteractionMap defaultCollisions() {
    CollisionInteractionMap collisionMap = new CollisionInteractionMap();

    collisionMap.onCollision(Player.class, Ghost.class,
        (player, ghost) -> player.setAlive(false));

    collisionMap.onCollision(Player.class, Pellet.class,
        (player, pellet) -> {
            pellet.leaveSquare();
            player.addPoints(pellet.getValue());
        });
    return collisionMap;
```
This method determines what happens when the player collides with a pellet; in which case will call 2 methods that will first make the pellet leave the square, then add points to the player. 
Similarly, we can parameterize a fruit class when the player collides with a fruit.

Pellets also occur in the Level.java class, which implies that Level.java class should also be changed to add fruit into the game. 
```
public int remainingPellets() {
    Board board = getBoard();
    int pellets = 0;
    for (int x = 0; x < board.getWidth(); x++) {
        for (int y = 0; y < board.getHeight(); y++) {
            for (Unit unit : board.squareAt(x, y).getOccupants()) {
                if (unit instanceof Pellet) {
                    pellets++;
                }
            }
        }
    }
    assert pellets >= 0;
    return pellets;
}
```
The remainingPellets() method creates an instance of the board and counts the number of pellets remaining on the board. To count the number of fruits left on the board, we should also create a method alike. 

As mentioned in the beginning, fruits are just fancier pellets, which means they are likely to be worth more points. To set the default value of a fruit, we must change the LevelFactory.java file, where we have also set the default value of a pellet. 

```
private static final int PELLET_VALUE = 10;
```
Lastly, in order to get the fruit sprite, create a new fruit, and append it on the game board, we will need to edit PacManSprites.java, Pellet.java, and MapParser.java files respectively. 
