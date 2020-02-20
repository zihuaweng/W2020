# Assignment Answer 01/16/2020

## Q1: What is the role of EmptySprite?
According to _EmptySprite.java_, EmptySprite implements Sprite but does not contain any data. Nothing happens when this sprite is drawn. It's used in two classes: AnimatedSprite and ImageSprite.

In _AnimatedSprite.java_, a new EmptySprite is assigned to a static final Sprite called _END\_OF\_LOOP_. It simply serves as the end of a non-looping sprite in this case. When the animation ends, the EmptySprite is drawn.

```java
/**
 * Static empty sprite to serve as the end of a non-looping sprite.
 */
private static final Sprite END_OF_LOOP = new EmptySprite();
```
```java
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
In the _split_ function  of _ImageSprite.java_, EmptySprite is returned when the region is not in the current sprite. In other words, this function uses EmptySprite to avoid actually drawing sprites that have coordinates beyond the coordinate range of the image.

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


## Q2: What is the role of MOVE\_INTERVAL and INTERVAL\_VARIATION?

MOVE\_INTERVAL is the value assigned to the variable _moveInterval_ when a subclass of _Ghost_ is initialized. At the same time, similarly, INTERVAL\_VARIATION is the value assigned to the variable _intervalVariation_. Both _moveInterval_ and _intervalVariation_ are documented in detail in _Ghost.java_.

_moveInterval_ is the base move interval of the ghost. _intervalVariation_ is the random variation added to the _moveInterval_. The value of the suggested delay time between moves in milliseconds equals the value of _moveInterval_ plus a random value ranged from 0 to the value of _intervalVariation_. 

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

In this way, ghosts' move behavior can be more random and hard to predict.


## Q3: If you wanted to add a fruit, which files would you need to change?

Files that should be modified:

* _board.txt_
* _MapParser.java_
* _LevelFactory.java_
* _PacManSprites.java_

In _Launcher.java_, I find information about the default map for this game is stored in _board.txt_. Then after finding where the usage of _levelMap_ is, _getMapParser()_ function reminds me of exploring the _MapParser_ class.

```java
/**
 * The map file used to populate the level.
 *
 * @return The name of the map file.
 */
protected String getLevelMap() {
    return levelMap;
}

/**
 * Creates a new level. By default this method will use the map parser to
 * parse the default board stored in the <code>board.txt</code> resource.
 *
 * @return A new level.
 */
public Level makeLevel() {
    try {
        return getMapParser().parseMap(getLevelMap());
    } catch (IOException e) {
        throw new PacmanConfigurationException(
                "Unable to create level, name = " + getLevelMap(), e);
    }
}

/**
 * @return A new map parser object using the factories from
 *         {@link #getLevelFactory()} and {@link #getBoardFactory()}.
 */
protected MapParser getMapParser() {
    return new MapParser(getLevelFactory(), getBoardFactory());
}
```

In _MapParser.java_, I find the _parseMap_ function is used to parse the text representation of the board into an actual level. It supports 5 kinds of characters: ' ' for an empty square, '#' for a wall, '.' for a square with a pellet, 'P' for a starting square for players, 'G' for a starting square with a ghost. Base on that I think we need to add some other characters to represent different kind of fruits. The solution is to add cases in _addSquare()_ function. For example, we can use 'M' for a melon.

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
        case 'G':
            Square ghostSquare = makeGhostSquare(ghosts, levelCreator.createGhost());
            grid[x][y] = ghostSquare;
            break;
        case 'P':
            Square playerSquare = boardCreator.createGround();
            grid[x][y] = playerSquare;
            startPositions.add(playerSquare);
            break;
        case 'M': 
            Square fruitSquare = boardCreator.createGround();
            grid[x][y] = fruitSquare;
            levelCreator.createFruit().occupy(fruitSquare);
            break;
            ...
        default:
            throw new PacmanConfigurationException("Invalid character at "
                + x + "," + y + ": " + c);
    }
}
```

On top of that, we need to a create new method called _createFruit(String fruit)_ for create fruits. it should be defined in _LevelFactory.java_. We also need a new int variable for the number of points of a fruit in the same file.

```java
private static final int FRUIT_VALUE = 50;

public Pellet createFruit(String fruit) {
    return new Pellet(FRUIT_VALUE, sprites.getFruitSprite(fruit));
}
```

Since _getPelletSprite()_ is called when a pellet square is created, we need to create a similar method to get a fruit sprite in _PacManSprites.java_. There are several kinds of fruits and each kind of fruits should be represented by one kind of Sprites so a new method called _getFruitSquare(String fruit)_ should be added with an extra parameter fruit to indicate which image from resource files should be loaded. Basically, the code is like:

```java
public Sprite getFruitSprite(String fruit) {
    return loadSprite("/sprite/" + fruit + ".png");
}
```