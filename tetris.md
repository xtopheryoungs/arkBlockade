# Tetris

## Step 1

Fly up into the air, then use the sword to start the game.  Look left/right of the board to move the falling pieces left/right.

Break the glass blocks to rotate the falling pieces.  Throw a snowball to send a falling piece straight down very quickly.

```template
function showPreviewBlock (block: number) {
    blocks.clone(
    positions.add(
    BOARD_ORIGIN,
    pos(1, 1, -1 * (block + 1))
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(TILE_BUFFER, TILE_BUFFER, -1 * (block + 1))
    ),
    PREVIEW_ANCHOR,
    CloneMask.Replace,
    CloneMode.Normal
    )
}
function defineS () {
    return [
    [
    1 * BOARD_WIDTH + 0,
    1 * BOARD_WIDTH + 1,
    2 * BOARD_WIDTH + 1,
    2 * BOARD_WIDTH + 2
    ],
    [
    0 * BOARD_WIDTH + 2,
    1 * BOARD_WIDTH + 1,
    1 * BOARD_WIDTH + 2,
    2 * BOARD_WIDTH + 1
    ],
    [
    0 * BOARD_WIDTH + 0,
    0 * BOARD_WIDTH + 1,
    1 * BOARD_WIDTH + 1,
    1 * BOARD_WIDTH + 2
    ],
    [
    0 * BOARD_WIDTH + 1,
    1 * BOARD_WIDTH + 0,
    1 * BOARD_WIDTH + 1,
    2 * BOARD_WIDTH + 0
    ]
    ]
}
function cleanUp () {
    player.execute(
    "clear"
    )
    i = 0
    for (let index = 0; index < BOARD_HEIGHT; index++) {
        blocks.fill(
        GAME_OVER_MATERIAL,
        positions.add(
        BOARD_ORIGIN,
        pos(1, i + 1, 0)
        ),
        positions.add(
        BOARD_ORIGIN,
        pos(BOARD_WIDTH, i + 1, 0)
        ),
        FillOperation.Replace
        )
        i += 1
    }
    blocks.fill(
    AIR,
    positions.add(
    BOARD_ORIGIN,
    pos(1, BOARD_HEIGHT + 1, 0)
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(BOARD_WIDTH, BOARD_HEIGHT + 1, 0)
    ),
    FillOperation.Replace
    )
    loops.pause(3000)
    blocks.fill(
    AIR,
    positions.add(
    BOARD_ORIGIN,
    pos(TILE_BUFFER * -1, 0, 2)
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(BOARD_WIDTH + 1, BOARD_HEIGHT + 5, (N_TETROMINOES + 3) * -1)
    ),
    FillOperation.Replace
    )
    blocks.fill(
    AIR,
    positions.add(
    BUTTONS_ANCHOR,
    pos(-1, 0, 0)
    ),
    positions.add(
    BUTTONS_ANCHOR,
    pos(1, 1, 0)
    ),
    FillOperation.Replace
    )
    mobs.give(
    mobs.target(NEAREST_PLAYER),
    DIAMOND_SWORD,
    1
    )
}
function modulo (dividend: number, divisor: number) {
    return dividend - divisor * Math.floor(dividend / divisor)
}
function getLookDirection (rawDirection: number) {
    if (Math.abs(rawDirection) >= 168) {
        return pos(0, 0, 0)
    } else {
        return pos(-1 * Math.idiv(Math.abs(rawDirection), rawDirection), 0, 0)
    }
}
player.onItemInteracted(SNOWBALL, function () {
    player.execute(
    "clear"
    )
    autoDown = true
})
player.onItemInteracted(DIAMOND_SWORD, function () {
    player.runChatCommand("run")
})
function defineL () {
    return [
    [
    1 * BOARD_WIDTH + 0,
    1 * BOARD_WIDTH + 1,
    1 * BOARD_WIDTH + 2,
    2 * BOARD_WIDTH + 2
    ],
    [
    0 * BOARD_WIDTH + 1,
    0 * BOARD_WIDTH + 2,
    1 * BOARD_WIDTH + 1,
    2 * BOARD_WIDTH + 1
    ],
    [
    0 * BOARD_WIDTH + 0,
    1 * BOARD_WIDTH + 0,
    1 * BOARD_WIDTH + 1,
    1 * BOARD_WIDTH + 2
    ],
    [
    0 * BOARD_WIDTH + 1,
    1 * BOARD_WIDTH + 1,
    2 * BOARD_WIDTH + 0,
    2 * BOARD_WIDTH + 1
    ]
    ]
}
function blockCanMove (boardState: any[], tetromino: any[], pos2: Position, dir: Position) {
    for (let value of tetromino) {
        x = pos2.getValue(Axis.X) + 1 + value % BOARD_WIDTH
        y = pos2.getValue(Axis.Y) + 1 + Math.idiv(value, BOARD_WIDTH)
        if (boardState[y * GRID_WIDTH + x + dir.getValue(Axis.X)] || boardState[y * GRID_WIDTH + x + dir.getValue(Axis.Y) * GRID_WIDTH]) {
            return false
        }
    }
    return true
}
function defineO () {
    return [
    [
    1 * BOARD_WIDTH + 1,
    1 * BOARD_WIDTH + 2,
    2 * BOARD_WIDTH + 1,
    2 * BOARD_WIDTH + 2
    ],
    [
    1 * BOARD_WIDTH + 1,
    1 * BOARD_WIDTH + 2,
    2 * BOARD_WIDTH + 1,
    2 * BOARD_WIDTH + 2
    ],
    [
    1 * BOARD_WIDTH + 1,
    1 * BOARD_WIDTH + 2,
    2 * BOARD_WIDTH + 1,
    2 * BOARD_WIDTH + 2
    ],
    [
    1 * BOARD_WIDTH + 1,
    1 * BOARD_WIDTH + 2,
    2 * BOARD_WIDTH + 1,
    2 * BOARD_WIDTH + 2
    ]
    ]
}
function lineIsFull (boardState: any[], row: number) {
    i = 0
    for (let index = 0; index < GRID_WIDTH; index++) {
        if (!(boardState[row * GRID_WIDTH + i])) {
            return false
        }
        i += 1
    }
    return true
}
function defineT () {
    return [
    [
    1 * BOARD_WIDTH + 0,
    1 * BOARD_WIDTH + 1,
    1 * BOARD_WIDTH + 2,
    2 * BOARD_WIDTH + 1
    ],
    [
    0 * BOARD_WIDTH + 1,
    1 * BOARD_WIDTH + 1,
    1 * BOARD_WIDTH + 2,
    2 * BOARD_WIDTH + 1
    ],
    [
    0 * BOARD_WIDTH + 1,
    1 * BOARD_WIDTH + 0,
    1 * BOARD_WIDTH + 1,
    1 * BOARD_WIDTH + 2
    ],
    [
    0 * BOARD_WIDTH + 1,
    1 * BOARD_WIDTH + 0,
    1 * BOARD_WIDTH + 1,
    2 * BOARD_WIDTH + 1
    ]
    ]
}
player.onChat("run", function () {
    player.execute(
    "clear"
    )
    player.execute(
    "tp @p ~0 ~0 ~0 180 0"
    )
    ORIGIN = player.position()
    BOARD_ORIGIN = positions.add(
    ORIGIN,
    pos(Math.idiv(BOARD_WIDTH, -2), Math.idiv(BOARD_WIDTH, -2), Z_DISTANCE)
    )
    PREVIEW_ANCHOR = positions.add(
    BOARD_ORIGIN,
    pos(TILE_BUFFER * -1, BOARD_HEIGHT - TILE_BUFFER, 0)
    )
    BUTTON_TEMPLATE_ANCHOR = positions.add(
    BOARD_ORIGIN,
    pos(TILE_BUFFER + 1, 0, -1)
    )
    BUTTONS_ANCHOR = positions.add(
    ORIGIN,
    pos(0, 1, -5)
    )
    grid = []
    i = 0
    for (let index = 0; index < GRID_HEIGHT; index++) {
        j = 0
        for (let index = 0; index < GRID_WIDTH; index++) {
            grid.push(j == 0 || j == GRID_WIDTH - 1)
            if (i == 0) {
                grid[j] = !(grid.pop())
            }
            j += 1
        }
        i += 1
    }
    blockBag = makeFullBlockBag()
    setup(BOARD_ORIGIN)
    gameOver = false
    previewBlockIndex = blockBag.removeAt(randint(0, blockBag.length - 1))
    while (!(grid[GAME_OVER_INDEX])) {
        blockIndex = previewBlockIndex
        rotationIndex = 0
        relativeBlockPosition = SPAWN_POINT_OFFSET
        absoluteBlockPosition = positions.add(
        BOARD_ORIGIN,
        relativeBlockPosition
        )
        previewBlockIndex = blockBag.removeAt(randint(0, blockBag.length - 1))
        if (blockBag.length == 0) {
            blockBag = makeFullBlockBag()
        }
        showPreviewBlock(previewBlockIndex)
        copyBlockToBoard(blockIndex, rotationIndex, absoluteBlockPosition)
        autoDown = false
        mobs.give(
        mobs.target(NEAREST_PLAYER),
        SNOWBALL,
        1
        )
        while (blockCanMove(grid, TETRIS_BLOCKS[blockIndex][rotationIndex], relativeBlockPosition, UNIT_DOWN)) {
            counter = 8
            while (counter > 0) {
                if (blockCanMove(grid, TETRIS_BLOCKS[blockIndex][rotationIndex], relativeBlockPosition, UNIT_DOWN)) {
                    relativeBlockPosition = positions.add(
                    relativeBlockPosition,
                    UNIT_DOWN
                    )
                    absoluteBlockPosition = positions.add(
                    absoluteBlockPosition,
                    UNIT_DOWN
                    )
                }
                copyBlockToBoard(blockIndex, rotationIndex, absoluteBlockPosition)
                counter += -2
                if (!(autoDown)) {
                    lookDirection = getLookDirection(Math.round(player.getOrientation()))
                    if (lookDirection.getValue(Axis.X) != 0) {
                        if (blockCanMove(grid, TETRIS_BLOCKS[blockIndex][rotationIndex], relativeBlockPosition, lookDirection)) {
                            relativeBlockPosition = positions.add(
                            relativeBlockPosition,
                            lookDirection
                            )
                            absoluteBlockPosition = positions.add(
                            absoluteBlockPosition,
                            lookDirection
                            )
                            copyBlockToBoard(blockIndex, rotationIndex, absoluteBlockPosition)
                            counter += -2
                        }
                    }
                    if (!(blocks.testForBlock(BUTTON_MATERIAL, positions.add(
                    BUTTONS_ANCHOR,
                    UNIT_LEFT
                    )))) {
                        if (blockCanMove(grid, TETRIS_BLOCKS[blockIndex][modulo(rotationIndex - 1, N_ROTATIONS)], relativeBlockPosition, pos(0, 0, 0))) {
                            rotationIndex = modulo(rotationIndex - 1, N_ROTATIONS)
                            copyBlockToBoard(blockIndex, rotationIndex, absoluteBlockPosition)
                            restoreController()
                            counter += -3
                        }
                    }
                    if (!(blocks.testForBlock(BUTTON_MATERIAL, positions.add(
                    BUTTONS_ANCHOR,
                    UNIT_RIGHT
                    )))) {
                        if (blockCanMove(grid, TETRIS_BLOCKS[blockIndex][modulo(rotationIndex + 1, N_ROTATIONS)], relativeBlockPosition, pos(0, 0, 0))) {
                            rotationIndex = modulo(rotationIndex + 1, N_ROTATIONS)
                            copyBlockToBoard(blockIndex, rotationIndex, absoluteBlockPosition)
                            restoreController()
                            counter += -3
                        }
                    }
                } else {
                    counter = 0
                }
            }
        }
        setBlockInBoard(absoluteBlockPosition, MINECRAFT_BLOCKS[blockIndex])
        for (let value of TETRIS_BLOCKS[blockIndex][rotationIndex]) {
            x = relativeBlockPosition.getValue(Axis.X) + 1 + value % BOARD_WIDTH
            y = relativeBlockPosition.getValue(Axis.Y) + 1 + Math.idiv(value, BOARD_WIDTH)
            grid[y * GRID_WIDTH + x] = true
        }
        linesToRemove = []
        j = BOARD_HEIGHT
        for (let index = 0; index < BOARD_HEIGHT; index++) {
            if (lineIsFull(grid, j)) {
                linesToRemove.push(j)
                for (let index = 0; index < GRID_WIDTH; index++) {
                    grid.removeAt(j * GRID_WIDTH)
                }
                prepareToRemoveLine(j)
                k = 0
                for (let index = 0; index < GRID_WIDTH; index++) {
                    if (k == 0 || k == GRID_WIDTH - 1) {
                        grid.push(true)
                    } else {
                        grid.push(false)
                    }
                    k += 1
                }
            }
            j += -1
        }
        if (linesToRemove.length > 0) {
            removeLines(linesToRemove)
        }
    }
    copyBlockToBoard(previewBlockIndex, rotationIndex, absoluteBlockPosition)
    setBlockInBoard(positions.add(
    BOARD_ORIGIN,
    SPAWN_POINT_OFFSET
    ), MINECRAFT_BLOCKS[previewBlockIndex])
    cleanUp()
})
function defineJ () {
    return [
    [
    1 * BOARD_WIDTH + 0,
    1 * BOARD_WIDTH + 1,
    1 * BOARD_WIDTH + 2,
    2 * BOARD_WIDTH + 0
    ],
    [
    0 * BOARD_WIDTH + 1,
    1 * BOARD_WIDTH + 1,
    2 * BOARD_WIDTH + 1,
    2 * BOARD_WIDTH + 2
    ],
    [
    0 * BOARD_WIDTH + 2,
    1 * BOARD_WIDTH + 0,
    1 * BOARD_WIDTH + 1,
    1 * BOARD_WIDTH + 2
    ],
    [
    0 * BOARD_WIDTH + 0,
    0 * BOARD_WIDTH + 1,
    1 * BOARD_WIDTH + 1,
    2 * BOARD_WIDTH + 1
    ]
    ]
}
function defineI () {
    return [
    [
    1 * BOARD_WIDTH + 0,
    1 * BOARD_WIDTH + 1,
    1 * BOARD_WIDTH + 2,
    1 * BOARD_WIDTH + 3
    ],
    [
    0 * BOARD_WIDTH + 1,
    1 * BOARD_WIDTH + 1,
    2 * BOARD_WIDTH + 1,
    3 * BOARD_WIDTH + 1
    ],
    [
    2 * BOARD_WIDTH + 0,
    2 * BOARD_WIDTH + 1,
    2 * BOARD_WIDTH + 2,
    2 * BOARD_WIDTH + 3
    ],
    [
    0 * BOARD_WIDTH + 2,
    1 * BOARD_WIDTH + 2,
    2 * BOARD_WIDTH + 2,
    3 * BOARD_WIDTH + 2
    ]
    ]
}
function setup (anchor: Position) {
    blocks.fill(
    SEA_LANTERN,
    anchor,
    positions.add(
    anchor,
    pos(1 + BOARD_WIDTH, BOARD_HEIGHT, 0)
    ),
    FillOperation.Replace
    )
    i = 0
    for (let index = 0; index < TETRIS_BLOCKS.length; index++) {
        j = 0
        for (let index = 0; index < TETRIS_BLOCKS[i].length; index++) {
            k = 0
            for (let index = 0; index < TETRIS_BLOCKS[i][j].length; index++) {
                coordinate = TETRIS_BLOCKS[i][j][k]
                blocks.place(MINECRAFT_BLOCKS[i], positions.add(
                BOARD_ORIGIN,
                pos(coordinate % BOARD_WIDTH + 1, Math.idiv(coordinate, BOARD_WIDTH) + TILE_BUFFER * j + 1, -1 - i)
                ))
                k += 1
            }
            j += 1
        }
        i += 1
    }
    blocks.fill(
    BOARD_MATERIAL,
    positions.add(
    anchor,
    pos(1, 1, 0)
    ),
    positions.add(
    anchor,
    pos(BOARD_WIDTH, BOARD_HEIGHT, 0)
    ),
    FillOperation.Replace
    )
    blocks.fill(
    BUTTON_MATERIAL,
    BUTTON_TEMPLATE_ANCHOR,
    positions.add(
    BUTTON_TEMPLATE_ANCHOR,
    BUTTON_TEMPLATE_OFFSET
    ),
    FillOperation.Replace
    )
    blocks.fill(
    AIR,
    positions.add(
    BUTTON_TEMPLATE_ANCHOR,
    pos(1, 0, 0)
    ),
    positions.add(
    BUTTON_TEMPLATE_ANCHOR,
    pos(1, 1, 0)
    ),
    FillOperation.Replace
    )
    restoreController()
}
function setBlockInBoard (pos2: Position, filterBlock: number) {
    blocks.cloneFiltered(
    pos2,
    positions.add(
    pos2,
    pos(TILE_BUFFER, TILE_BUFFER, 0)
    ),
    positions.add(
    pos2,
    pos(0, 0, -1)
    ),
    filterBlock,
    CloneMode.Move
    )
    blocks.clone(
    positions.add(
    BOARD_ORIGIN,
    pos(1, 1, 0)
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(BOARD_WIDTH, BOARD_HEIGHT, 0)
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(1, 1, (N_TETROMINOES + 1) * -1)
    ),
    CloneMask.Replace,
    CloneMode.Normal
    )
    blocks.clone(
    positions.add(
    BOARD_ORIGIN,
    pos(1, 1, 0)
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(BOARD_WIDTH, BOARD_HEIGHT, 0)
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(1, 1, (N_TETROMINOES + 3) * -1)
    ),
    CloneMask.Replace,
    CloneMode.Normal
    )
}
function restoreController () {
    blocks.clone(
    BUTTON_TEMPLATE_ANCHOR,
    positions.add(
    BUTTON_TEMPLATE_ANCHOR,
    BUTTON_TEMPLATE_OFFSET
    ),
    positions.add(
    BUTTONS_ANCHOR,
    pos(-1, 0, 0)
    ),
    CloneMask.Replace,
    CloneMode.Normal
    )
}
function copyBlockToBoard (block: number, rotation: number, pos2: Position) {
    blocks.clone(
    positions.add(
    BOARD_ORIGIN,
    pos(0, rotation * TILE_BUFFER, -1 * (block + 1))
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(TILE_BUFFER, (rotation + 1) * TILE_BUFFER, -1 * (block + 1))
    ),
    positions.add(
    pos2,
    pos(0, 0, 0)
    ),
    CloneMask.Replace,
    CloneMode.Normal
    )
}
function makeFullBlockBag () {
    list = []
    i = 0
    for (let index = 0; index < N_TETROMINOES; index++) {
        list.push(i)
        i += 1
    }
    return list
}
function delay (ticks: number) {
    for (let index = 0; index < ticks; index++) {
        if (blocks.testForBlock(GRASS, pos(0, 0, 0))) {
        	
        }
    }
}
function removeLines (lineNumbers: any[]) {
    blocks.clone(
    positions.add(
    BOARD_ORIGIN,
    pos(1, 1, 0)
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(BOARD_WIDTH, BOARD_HEIGHT, 0)
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(1, 1, (N_TETROMINOES + 2) * -1)
    ),
    CloneMask.Replace,
    CloneMode.Normal
    )
    for (let index = 0; index < 3; index++) {
        delay(3)
        blocks.clone(
        positions.add(
        BOARD_ORIGIN,
        pos(1, 1, (N_TETROMINOES + 2) * -1)
        ),
        positions.add(
        BOARD_ORIGIN,
        pos(BOARD_WIDTH, BOARD_HEIGHT, (N_TETROMINOES + 2) * -1)
        ),
        positions.add(
        BOARD_ORIGIN,
        pos(1, 1, 0)
        ),
        CloneMask.Replace,
        CloneMode.Normal
        )
        delay(3)
        blocks.clone(
        positions.add(
        BOARD_ORIGIN,
        pos(1, 1, (N_TETROMINOES + 1) * -1)
        ),
        positions.add(
        BOARD_ORIGIN,
        pos(BOARD_WIDTH, BOARD_HEIGHT, (N_TETROMINOES + 1) * -1)
        ),
        positions.add(
        BOARD_ORIGIN,
        pos(1, 1, 0)
        ),
        CloneMask.Replace,
        CloneMode.Normal
        )
    }
    deleteRows = lineNumbers
    deleteRows.reverse()
    i = 0
    for (let value of deleteRows) {
        blocks.clone(
        positions.add(
        BOARD_ORIGIN,
        pos(1, value + 1, 0)
        ),
        positions.add(
        BOARD_ORIGIN,
        pos(BOARD_WIDTH, BOARD_HEIGHT, 0)
        ),
        positions.add(
        BOARD_ORIGIN,
        pos(1, value - i, (N_TETROMINOES + 2) * -1)
        ),
        CloneMask.Replace,
        CloneMode.Normal
        )
        i += 1
    }
    blocks.clone(
    positions.add(
    BOARD_ORIGIN,
    pos(1, 1, (N_TETROMINOES + 3) * -1)
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(BOARD_WIDTH, BOARD_HEIGHT, (N_TETROMINOES + 3) * -1)
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(1, 1, 0)
    ),
    CloneMask.Replace,
    CloneMode.Move
    )
    delay(3)
    blocks.fill(
    BOARD_MATERIAL,
    positions.add(
    BOARD_ORIGIN,
    pos(1, BOARD_HEIGHT - i, (N_TETROMINOES + 2) * -1)
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(BOARD_WIDTH, BOARD_HEIGHT, (N_TETROMINOES + 2) * -1)
    ),
    FillOperation.Replace
    )
    blocks.clone(
    positions.add(
    BOARD_ORIGIN,
    pos(1, 1, (N_TETROMINOES + 2) * -1)
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(BOARD_WIDTH, BOARD_HEIGHT, (N_TETROMINOES + 2) * -1)
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(1, 1, 0)
    ),
    CloneMask.Replace,
    CloneMode.Move
    )
}
function prepareToRemoveLine (row: number) {
    blocks.fill(
    LINE_REMOVAL_MATERIAL,
    positions.add(
    BOARD_ORIGIN,
    pos(1, row, (N_TETROMINOES + 1) * -1)
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(BOARD_WIDTH, row, (N_TETROMINOES + 1) * -1)
    ),
    FillOperation.Replace
    )
    blocks.fill(
    BOARD_MATERIAL,
    positions.add(
    BOARD_ORIGIN,
    pos(1, row, (N_TETROMINOES + 3) * -1)
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(BOARD_WIDTH, row, (N_TETROMINOES + 3) * -1)
    ),
    FillOperation.Replace
    )
}
function defineZ () {
    return [
    [
    1 * BOARD_WIDTH + 1,
    1 * BOARD_WIDTH + 2,
    2 * BOARD_WIDTH + 0,
    2 * BOARD_WIDTH + 1
    ],
    [
    0 * BOARD_WIDTH + 1,
    1 * BOARD_WIDTH + 1,
    1 * BOARD_WIDTH + 2,
    2 * BOARD_WIDTH + 2
    ],
    [
    0 * BOARD_WIDTH + 1,
    0 * BOARD_WIDTH + 2,
    1 * BOARD_WIDTH + 0,
    1 * BOARD_WIDTH + 1
    ],
    [
    0 * BOARD_WIDTH + 0,
    1 * BOARD_WIDTH + 0,
    1 * BOARD_WIDTH + 1,
    2 * BOARD_WIDTH + 1
    ]
    ]
}
let deleteRows: number[] = []
let list: number[] = []
let coordinate = 0
let k = 0
let linesToRemove: number[] = []
let lookDirection: Position = null
let counter = 0
let absoluteBlockPosition: Position = null
let relativeBlockPosition: Position = null
let rotationIndex = 0
let blockIndex = 0
let previewBlockIndex = 0
let gameOver = false
let blockBag: number[] = []
let j = 0
let grid: boolean[] = []
let BUTTON_TEMPLATE_ANCHOR: Position = null
let ORIGIN: Position = null
let y = 0
let x = 0
let autoDown = false
let BUTTONS_ANCHOR: Position = null
let i = 0
let PREVIEW_ANCHOR: Position = null
let BOARD_ORIGIN: Position = null
let BUTTON_TEMPLATE_OFFSET: Position = null
let SPAWN_POINT_OFFSET: Position = null
let GAME_OVER_INDEX = 0
let UNIT_RIGHT: Position = null
let UNIT_LEFT: Position = null
let UNIT_DOWN: Position = null
let N_ROTATIONS = 0
let N_TETROMINOES = 0
let TETRIS_BLOCKS: number[][][] = []
let Z_DISTANCE = 0
let GRID_WIDTH = 0
let GRID_HEIGHT = 0
let BOARD_WIDTH = 0
let BOARD_HEIGHT = 0
let TILE_BUFFER = 0
let LINE_REMOVAL_MATERIAL = 0
let GAME_OVER_MATERIAL = 0
let BUTTON_MATERIAL = 0
let BOARD_MATERIAL = 0
let MINECRAFT_BLOCKS: number[] = []
MINECRAFT_BLOCKS = [
IRON_BLOCK,
GOLD_BLOCK,
DIAMOND_BLOCK,
EMERALD_BLOCK,
BLOCK_OF_NETHERITE,
POLISHED_ANDESITE,
LAPIS_LAZULI_BLOCK
]
BOARD_MATERIAL = ENDSTONE
BUTTON_MATERIAL = GLASS
GAME_OVER_MATERIAL = DIRT
LINE_REMOVAL_MATERIAL = SPONGE
TILE_BUFFER = 5
BOARD_HEIGHT = 20
BOARD_WIDTH = 10
GRID_HEIGHT = BOARD_HEIGHT + TILE_BUFFER
GRID_WIDTH = BOARD_WIDTH + 2
Z_DISTANCE = -30
TETRIS_BLOCKS = [
defineI(),
defineO(),
defineJ(),
defineL(),
defineS(),
defineZ(),
defineT()
]
N_TETROMINOES = TETRIS_BLOCKS.length
N_ROTATIONS = 4
UNIT_DOWN = pos(0, -1, 0)
UNIT_LEFT = pos(-1, 0, 0)
UNIT_RIGHT = pos(1, 0, 0)
GAME_OVER_INDEX = BOARD_HEIGHT * GRID_WIDTH + Math.idiv(BOARD_WIDTH, 2)
SPAWN_POINT_OFFSET = pos(Math.idiv(BOARD_WIDTH, 2) - 2, BOARD_HEIGHT - 2, 1)
BUTTON_TEMPLATE_OFFSET = pos(2, 0, 0)
player.execute(
"clear"
)
mobs.give(
mobs.target(NEAREST_PLAYER),
DIAMOND_SWORD,
1
)


```
    
