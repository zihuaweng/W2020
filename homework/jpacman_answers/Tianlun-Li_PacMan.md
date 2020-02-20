# JPacMan Homework Answer

## Q1:What is the role of EmptySprite?

EmptySprite is used in two files: `ImageSprite.java` and `AnimatedSprite.java`

* In `ImageSprite.java`, we create animation by spliting the static image into multiple frames. By calling split method shown below, we create frames of animated sprite out of one .png image. When our coordinate is out of the boundary of the image, we return a EmptySprite which draws nothing.

  ```java
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

* In `AnimatedSprite.java`, EmptySprite servers as the end of an non-looping animated sprite. We need to get the current frame of an animated sprite by calling the currentSprite method. For non-looping sprites, we use an EmptySprite to prevent the currentSprite method return null and keep `currentSprite.draw() ` always valid.

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

  



## Q2: What is the role of MOVE_INTERVAL and INTERVAL_VARIATION?

`MOVE_INTERVAL` is the base movement interval of the ghosts; 

`INTERVAL_VARIATION` is used to generate a random variation of movement interval in order to make the ghost movement more dynamic.

We can also customize the move interval for ghosts by changing the MOVE_INTERVAL and INTERVAL_VARIATION of each ghost.

The combined movement interval for ghosts are computed by the function below, located in `Ghost.java`

```java
public long getInterval() {
  return this.moveInterval + new Random().nextInt(this.intervalVariation);
}
```

## Q3: If you wanted to add a fruit, which files would you need to change?

Fruits are special pellets, but with higher value and different image, so we need to:

* Create a class `Fruit` which extend `Pellet`.

* Modify `LevelFactory.java` and add a `createFruit` method.
* Modify `MapParser.java` to add different types of square to represent fruites
* Modify `PlayerCollision.java` to handle the situation when pacman collides with different kinds of fruits
* Modify `PacManSprites.java` and add a `getFruitSprite` method to load fruit sprite from images

