## 1. what is the role of EmptySprite?
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
```
Found 1 usage of EmptySprite:
```java
/**
 * Static empty sprite to serve as the end of a non-looping sprite.
 */
private static final Sprite END_OF_LOOP = new EmptySprite();
```
what is `END_OF_LOOP`?
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
seems like the EmptySprite is used as a default here.

what is `current`?

```java
/**
 * Updates the current frame index depending on the current system time.
 */
private void update() {
    long now = System.currentTimeMillis();
    if (animating) {
        while (lastUpdate < now) {
            lastUpdate += animationDelay;
            current++;
            if (looping) {
                current %= animationFrames.length;
            } else if (current == animationFrames.length) {
                animating = false;
            }
        }
    } else {
        lastUpdate = now;
    }
}
```
seems like 'current' is a condition indicator in a loop.

what is `looping`?
```java
/**
 * Whether is animation should be looping or not.
 */
private final boolean looping;
```
There are also another 2 usage of EmptySprite:
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
if not within image, return an EmptySprite.
```java
@Test
public void splitOutOfBounds() {
    Sprite split = sprite.split(10, 10, 64, 10);
    assertThat(split).isInstanceOf(EmptySprite.class);
}
```
when out of bond, Sprite should be an EmptySprite.

#### Conclusion: `EmptySprite` is a class that used to complement `Sprite` interface to resolve (edge) cases and conditions that has not been taken into consideration.

## 2. what is the role of MOVE_INTERVAL and INTERVAL_VARIATION?
`MOVE_INTERVAL` and `INTERVAL_VARIATION` usage example:
```java
/**
 * The base movement interval.
 */
private static final int MOVE_INTERVAL = 250;
```
```java
/**
 * The variation in intervals, this makes the ghosts look more dynamic and
 * less predictable.
 */
private static final int INTERVAL_VARIATION = 50;
```

```java
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
#### Conclusion: `MOVE_INTERVAL` is used to set base movement interval of NPCs. `INTERVAL_VARIATION` is used to add variation upon NPCs' movements.


## 3. if you wanted to add a fruit, which files would you need to change?
A fruit is a special pellet. Therefore, similar to the implementation of pellets, files needs to be changed are:

## 3. if you wanted to add a fruit, which files would you need to change?
 A fruit is a special pellet. Therefore, similar to the implementation of pellets, files needs to be changed are:

 <br>1. There should be a `Fruit` class that extends `Unit`.
 <br>2. `LevelFactory.java` where new Pellet is created, and `MapParser` where `createPellet()` is used.
 <br>3. `PlayerCollisions.java` where function of pellet and player meet is defined.
 <br>4. files that related to collision: `DefaultPlayerInteractionMap.java`.
 <br>5. `Level.java` file that includes counting `remainingPellets`, and `Game.java` where `remainingPellets` is used.
 <br>6. and a picture for the fruit(which there are already some examples in the `sprite` folder).
 <br>7. `PacManSprites.java` where pictures are loaded.
