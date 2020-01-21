###### Assignment Duo Chai:

## what is the role of EmptySprite?

* As we can see, EmptySprite implents interface of sprite, so EmptySprite IS a sprite with 0 height and 0 weight, which also draws nothing and return another EmptySprite when being split.
```java
public class EmptySprite implements Sprite {...}
```

* By checking out its usage, we can see EmptySprite has been used in other two src classes and in one test case
    * AnimatedSprite: Any AnimatedSprite will result in an EmptySprite at the end of loop
    ```java
    private static final Sprite END_OF_LOOP = new EmptySprite();
    ```

    * ImageSprite: When splitting an ImageSprite, if not within image boundary, it will result in returning an EmptySprite.
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

    * In test case, it is being tested for the functionality of split method which will result in an EmptySprite when ImageSprite not being split within an actualy sprite.
    ```java
    @Test
    public void splitOutOfBounds() {
        Sprite split = sprite.split(10, 10, 64, 10);
        assertThat(split).isInstanceOf(EmptySprite.class);
    }        
    ```

## what is the role of MOVE_INTERVAL and INTERVAL_VARIATION?

* MOVE_INTERVAL, INTERVAL_VARIATION are defined in 4 npc (Blinky, Clyde, Inky, Pinky) ghost files. All ghost npc constructors call their parent's constructor by passing these two variables, which being set to be 250, 50 respectively.
```java
private static final int INTERVAL_VARIATION = 50;

private static final int MOVE_INTERVAL = 250;

public Blinky(Map<Direction, Sprite> spriteMap) {
    super(spriteMap, MOVE_INTERVAL, INTERVAL_VARIATION);
}
```

* getInterval() is used in startNPCs() method level class and decides ghosts' moving pace by returning a value base on a fixed movement value which is generated upon value of MOVE_INTERVAl + a random integer generated upon value of INTERVAL_VARIATION as a moving interval.
```java
public long getInterval() {
    return this.moveInterval + new Random().nextInt(this.intervalVariation);
}

private void startNPCs() {
    for (final Ghost npc : npcs.keySet()) {
        ScheduledExecutorService service = Executors.newSingleThreadScheduledExecutor();

        service.schedule(new NpcMoveTask(service, npc),
            npc.getInterval() / 2, TimeUnit.MILLISECONDS);

        npcs.put(npc, service);
    }
}        
```

* Based on analysis above, we can tell that value of MOVE_INTERVAL, INTERVAL_VARIATION variables decide how fast a npc-ghost moves.

## if you wanted to add a fruit, which files would you need to change?

* Fruit can be seen as a pellet with huge bonus and different image, so we can just make changes to every files that are in the usage of pellet.
* We treat fruit as a peer of pellet, create a fruit class within level folder
```java
public class Fruit extends Unit{...}
```
* add fruitColliding(), playerVersusFruit() methods in level file within level folder, 
```java
    private void fruitColliding(Fruit fruit, Unit collidedOn) {...}

    public void playerVersusFruit(Player player, Fruit fruit) {...} 
```

* add if statement in playerColliding() method in playerCollisions file.
```java
    private void playerColliding(Player player, Unit collidedOn) {
        if (collidedOn instanceof Ghost) {
            playerVersusGhost(player, (Ghost) collidedOn);
        }
        if (collidedOn instanceof Pellet) {
            playerVersusPellet(player, (Pellet) collidedOn);
        }
        if (colliedOn instanceof Fruit) {...}
    }
```

* add createFruit() method in levelFactory file within level folder
```java
    public Fruit createFruit() {...}   
```

* add fruitSquare in MapParser file
```java
            case 'F':
                Square fruitSquare = boardCreator.createGround();
                grid[x][y] = fruitSquare;
                levelCreator.createfruit().occupy(fruitSquare);
                break;
```

* add getFruitSprite() method in PacManSprite file within sprite folder.
```java
    public Sprite getFruitSprite() {
        return loadSprite("/sprite/{fruit}.png");
    }
```
