1. Role of `EmptySprite`
`EmptySprite` is an implementation of Sprite with zero height and width and that draws nothing.
It is used in the `AnimatedSprite` class to display nothing at the end of a non-looping animated sprite's cycle:
```java
    private static final Sprite END_OF_LOOP = new EmptySprite();
```
It is also returned in the `split` function of the `ImageSprite` class to return an empty sprite if the given dimensions are out of bounds:
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

2. Role of `MOVE_INTERVAl` and INTERVAL_VARIATION
`MOVE_INTERVAL` is the base interval between movements unique to each ghost NPC (Inky, Binky, Pinky, and Clyde).
`INTERVAL_VARIATION` is the maximum amount of variation applied to `MOVE_INTERVAL` to make NPCs appear more dynamic. `INTERVAL_VARIATION` is currently set to 50 for each NPC, but could be made unique if desired.
`INTERVAL_VARIATION` and `MOVE_INTERVAL` are passed to the constructor of `Ghost` objects. 
(i.e., Blinky's constructor:
```java
    public Blinky(Map<Direction, Sprite> spriteMap) {
        super(spriteMap, MOVE_INTERVAL, INTERVAL_VARIATION);
    }
```
calls `super`:
```java
    protected Ghost(Map<Direction, Sprite> spriteMap, int moveInterval, int intervalVariation) {
        this.sprites = spriteMap;
        this.intervalVariation = intervalVariation;
        this.moveInterval = moveInterval;
    }
```
These two value are used in tandem in the `Ghost` class' `getInterval` method:
```java
    public long getInterval() {
        return this.moveInterval + new Random().nextInt(this.intervalVariation);
    }
```
The `getInterval` method is then used in the `startNPCs` method of the `Level` class to schedule NPC movements:
```java
    private void startNPCs() {
        for (final Ghost npc : npcs.keySet()) {
            ScheduledExecutorService service = Executors.newSingleThreadScheduledExecutor();

            service.schedule(new NpcMoveTask(service, npc),
                npc.getInterval() / 2, TimeUnit.MILLISECONDS);

            npcs.put(npc, service);
        }
    }
```
)

3. Adding a fruit:
`Fruit` should be added as a class that inherits from `Unit` in the `level` directory, similar to `Pellet`. Depending on how the Pellet is expected to interact with other classes, it might be more desirable to have `Fruit` inherit from `Pellet.`
The fruit sprite must first be loaded in the `PacManSprites` class using its own getter function, similar to `getPelletSprite`:
```java
    public Sprite getPelletSprite() {
        return loadSprite("/sprite/pellet.png");
    }
```
Assuming that `Fruit` is not inheriting from `Pellet`, a function should be added to `Player Collisions` similar to the `pelletColliding` function:
```java
    private void pelletColliding(Pellet pellet, Unit collidedOn) {
        if (collidedOn instanceof Player) {
            playerVersusPellet((Player) collidedOn, pellet);
        }
    }
```
A 'createFruit' function should be added to the `LevelFactory` class similar to the `createPellet` method:
```java
    public Pellet createPellet() {
        return new Pellet(PELLET_VALUE, sprites.getPelletSprite());
    }
```
This function should be called as part of the `switch` statement in `addSquare` in MapParser:
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
            default:
                throw new PacmanConfigurationException("Invalid character at "
                    + x + "," + y + ": " + c);
        }
    }
```
In order to do that, a symbol for fruit, such as "F", should be chosen and added to the text representation for the board (`board.txt` in the `resources` directory).