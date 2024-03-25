# Snake

## Step 1

Fly up into the air, then throw the snowball to start a game of snake.  Be sure to face north before starting the game.

```template
function cleanUp () {
    blocks.fill(
    AIR_BLOCK,
    positions.add(
    ORIGIN,
    pos((BOARD_WIDTH + 1) * -1, (BOARD_HEIGHT + 1) * -1, DPAD_TEMPLATE_DISTANCE)
    ),
    positions.add(
    ORIGIN,
    pos(BOARD_WIDTH + 1, BOARD_HEIGHT + 1, BOARD_DISTANCE)
    ),
    FillOperation.Replace
    )
    buildDpad(DPAD_CENTER, AIR_BLOCK)
}
player.onChat("snake", function () {
    ORIGIN = player.position()
    DPAD_CENTER = positions.add(
    ORIGIN,
    pos(0, 1, DPAD_DISTANCE)
    )
    snake = []
    snakeLength = 2
    direction = UNIT_UP
    gameOver = false
    outOfBounds = false
    eatingSelf = false
    foodInSnake = false
    foodLocation = positions.add(
    ORIGIN,
    pos(randint(1, BOARD_WIDTH), randint(1, BOARD_HEIGHT), BOARD_DISTANCE)
    )
    buildBoard(BOARD_WIDTH, BOARD_HEIGHT, BACKGROUND_DISTANCE, BOARD_BLOCK)
    buildDpad(DPAD_CENTER, DPAD_BLOCK)
    buildDpad(positions.add(
    ORIGIN,
    pos(0, 0, DPAD_TEMPLATE_DISTANCE)
    ), DPAD_BLOCK)
    i = 0
    for (let index = 0; index < snakeLength; index++) {
        snake.unshift(positions.add(
        ORIGIN,
        pos(direction.getValue(Axis.X) * i, direction.getValue(Axis.Y) * i, BOARD_DISTANCE)
        ))
        blocks.place(SNAKE_BLOCK, snake[0])
        i += 1
    }
    blocks.place(FOOD_BLOCK, foodLocation)
    while (!(gameOver)) {
        direction = processInput(direction)
        snake.unshift(positions.add(
        snake[0],
        direction
        ))
        i = 1
        for (let index = 0; index < snakeLength - 1; index++) {
            if (sameLocation(snake[0], snake[i])) {
                eatingSelf = true
            }
            i += 1
        }
        outOfBounds = Math.abs(snake[0].getValue(Axis.X) - ORIGIN.getValue(Axis.X)) > BOARD_WIDTH || Math.abs(snake[0].getValue(Axis.Y) - ORIGIN.getValue(Axis.Y)) > BOARD_HEIGHT
        if (outOfBounds || eatingSelf) {
            moveBlock(snake.pop(), snake[0])
            gameOver = true
        } else if (sameLocation(snake[0], foodLocation)) {
            blocks.place(SNAKE_BLOCK, snake[0])
            foodInSnake = true
            snakeLength += 1
            while (foodInSnake) {
                foodLocation = positions.add(
                ORIGIN,
                pos(randint(BOARD_WIDTH * -1, BOARD_WIDTH), randint(BOARD_HEIGHT * -1, BOARD_HEIGHT), BOARD_DISTANCE)
                )
                foodInSnake = false
                i = 0
                for (let index = 0; index < snakeLength; index++) {
                    if (sameLocation(snake[i], foodLocation)) {
                        foodInSnake = true
                    }
                    i += 1
                }
            }
            blocks.place(FOOD_BLOCK, foodLocation)
        } else {
            moveBlock(snake.pop(), snake[0])
        }
    }
    replaceDpad()
    loops.pause(2000)
    cleanUp()
    mobs.give(
    mobs.target(NEAREST_PLAYER),
    SNOWBALL,
    1
    )
})
function replaceDpad () {
    blocks.clone(
    positions.add(
    positions.add(
    ORIGIN,
    pos(0, 0, DPAD_TEMPLATE_DISTANCE)
    ),
    positions.add(
    UNIT_DOWN,
    UNIT_LEFT
    )
    ),
    positions.add(
    positions.add(
    ORIGIN,
    pos(0, 0, DPAD_TEMPLATE_DISTANCE)
    ),
    positions.add(
    UNIT_UP,
    UNIT_RIGHT
    )
    ),
    positions.add(
    DPAD_CENTER,
    positions.add(
    UNIT_DOWN,
    UNIT_LEFT
    )
    ),
    CloneMask.Replace,
    CloneMode.Normal
    )
}
player.onItemInteracted(SNOWBALL, function () {
    player.runChatCommand("snake")
    player.execute(
    "clear"
    )
})
function sameLocation (a: Position, b: Position) {
    if (a.getValue(Axis.X) == b.getValue(Axis.X)) {
        if (a.getValue(Axis.Y) == b.getValue(Axis.Y)) {
            if (a.getValue(Axis.Z) == b.getValue(Axis.Z)) {
                return true
            }
        }
    }
    return false
}
function buildDpad (center: Position, material: number) {
    blocks.place(material, positions.add(
    center,
    UNIT_LEFT
    ))
    blocks.place(material, positions.add(
    center,
    UNIT_RIGHT
    ))
    blocks.place(material, positions.add(
    center,
    UNIT_UP
    ))
    blocks.place(material, positions.add(
    center,
    UNIT_DOWN
    ))
}
function moveBlock (_from: Position, to: Position) {
    blocks.clone(
    _from,
    _from,
    to,
    CloneMask.Replace,
    CloneMode.Move
    )
}
function buildBoard (w: number, h: number, d: number, material: number) {
    blocks.fill(
    material,
    positions.add(
    ORIGIN,
    pos(w * -1, h * -1, d)
    ),
    positions.add(
    ORIGIN,
    pos(w, h, d)
    ),
    FillOperation.Replace
    )
}
function processInput (currentDirection: Position) {
    if (currentDirection.getValue(Axis.X) == 0) {
        if (blocks.testForBlock(AIR_BLOCK, positions.add(
        DPAD_CENTER,
        UNIT_LEFT
        ))) {
            replaceDpad()
            return UNIT_LEFT
        } else if (blocks.testForBlock(AIR_BLOCK, positions.add(
        DPAD_CENTER,
        UNIT_RIGHT
        ))) {
            replaceDpad()
            return UNIT_RIGHT
        }
    } else {
        if (blocks.testForBlock(AIR_BLOCK, positions.add(
        DPAD_CENTER,
        UNIT_UP
        ))) {
            replaceDpad()
            return UNIT_UP
        } else if (blocks.testForBlock(AIR_BLOCK, positions.add(
        DPAD_CENTER,
        UNIT_DOWN
        ))) {
            replaceDpad()
            return UNIT_DOWN
        }
    }
    return currentDirection
}
let i = 0
let foodLocation: Position = null
let foodInSnake = false
let eatingSelf = false
let outOfBounds = false
let gameOver = false
let direction: Position = null
let snakeLength = 0
let snake: Position[] = []
let DPAD_CENTER: Position = null
let ORIGIN: Position = null
let FOOD_BLOCK = 0
let SNAKE_BLOCK = 0
let DPAD_BLOCK = 0
let BOARD_BLOCK = 0
let AIR_BLOCK = 0
let UNIT_LEFT: Position = null
let UNIT_RIGHT: Position = null
let UNIT_DOWN: Position = null
let UNIT_UP: Position = null
let DPAD_DISTANCE = 0
let BOARD_DISTANCE = 0
let DPAD_TEMPLATE_DISTANCE = 0
let BACKGROUND_DISTANCE = 0
let BOARD_HEIGHT = 0
let BOARD_WIDTH = 0
BOARD_WIDTH = 17
BOARD_HEIGHT = 12
BACKGROUND_DISTANCE = -25
DPAD_TEMPLATE_DISTANCE = BACKGROUND_DISTANCE - 1
BOARD_DISTANCE = BACKGROUND_DISTANCE + 1
DPAD_DISTANCE = -4
UNIT_UP = pos(0, 1, 0)
UNIT_DOWN = pos(0, -1, 0)
UNIT_RIGHT = pos(1, 0, 0)
UNIT_LEFT = pos(-1, 0, 0)
AIR_BLOCK = AIR
BOARD_BLOCK = OBSIDIAN
DPAD_BLOCK = GLASS
SNAKE_BLOCK = EMERALD_BLOCK
FOOD_BLOCK = REDSTONE_BLOCK
player.execute(
"clear"
)
mobs.give(
mobs.target(NEAREST_PLAYER),
SNOWBALL,
1
)

```
    
