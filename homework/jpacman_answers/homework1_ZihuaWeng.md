## Homework 1
### 1. what is the role of EmptySprite? 

EmptySprite is used to initiate the drawing of sprite for the following cases:
- when the sprite index reaches the last one (which means reach the end of the loop), EmptySprite return it to the initial state.
- when pixel (x,y) is out of sprit image, EmptySprite return initial state.

```Java
/**
 * Static empty sprite to serve as the end of a non-looping sprite.
 */
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
```Java
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

### 2. what is the role of MOVE_INTERVAL and INTERVAL_VARIATION? 
MOVE_INTERVAL and INTERVAL_VARIATION are used to initiate the NPCs (4 ghosts). MOVE_INTERVAL is the base move interval of the ghost,INTERVAL_VARIATION is the random variation added to the interval which makes the ghosts look more dynamic and less predictable.

- The calculation of the interval
```Java
/**
 * The time that should be taken between moves.
 *
 * @return The suggested delay between moves in milliseconds.
 */
public long getInterval() {
    return this.moveInterval + new Random().nextInt(this.intervalVariation);
}
```
- The NPCs initiate with their own interval.
```Java
/**
 * Starts all NPC movement scheduling.
 */
private void startNPCs() {
    for (final Ghost npc : npcs.keySet()) {
        ScheduledExecutorService service = Executors.newSingleThreadScheduledExecutor();

        service.schedule(new NpcMoveTask(service, npc),
            npc.getInterval() / 2, TimeUnit.MILLISECONDS);

        npcs.put(npc, service);
    }
}
```

### 3. if you wanted to add a fruit, which files would you need to change? 
A fruit could be as a specific pellet with different pellet value and different sprite. So we could add fruit using the pellet code.
#### jpacman/sprite/PacManSprites.java
Here we add the fruit sprite which will help show the apple image. For example here we add apple.
```Java
    public Sprite getFruitSprite() {
        return loadSprite("/sprite/apple.png");
    }
```
#### jpacman/level/LevelFactory.java
Here we create fruit as a pellet but has a different value and sprite.
```Java
    private static final int PELLET_VALUE = 10;
    private static final int FRUIT_VALUE = 50;
    
    public Pellet createPellet() {
        return new Pellet(PELLET_VALUE, sprites.getPelletSprite());
    }
    
    public Pellet createFruit() {
        return new Pellet(PELLET_VALUE, sprites.getFruitSprite());
    }
```
#### board.txt
Next, we need to change the layout of the maze and add location of fruit on the map. A indicates apple on the map.
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
#..A..#....#....#.....#    
#.########.#.########.#    
#.....................#    
#######################    


#### jpacman/level/MapParser.java
Add createFruit() to MapParser.
```Java
protected void addSquare(Square[][] grid, List<Ghost> ghosts,
                             List<Square> startPositions, int x, int y, char c) {
        switch (c) {
            case ' ':
                grid[x][y] = boardCreator.createGround();
                break;
            .......
            case 'A':
                Square pelletSquare1 = boardCreator.createGround();
                grid[x][y] = pelletSquare1;
                levelCreator.createFruit().occupy(pelletSquare1);
                break;  
```           

