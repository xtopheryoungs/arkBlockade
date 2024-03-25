# Panel de Pon

## Step 1

Use the sword to start a new game.  Throw a snowball to swap the blocks highlighted by the cursor.

Pro-tip: play with a third-person camera view.

```template
function cleanUp () {
    blocks.fill(
    AIR,
    positions.add(
    BOARD_ORIGIN,
    pos(0, CURSOR_LAYER_OFFSET, -1)
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(GRID_WIDTH - 1, DUPE_LAYER_OFFSET - 2, BOARD_HEIGHT + 1)
    ),
    FillOperation.Replace
    )
}
player.onItemInteracted(SNOWBALL, function () {
    player.runChatCommand("swap")
})
function isValidIndexAndValueEqualTo (n: number, arr: number[], target: number) {
    if (n >= 0 && n < arr.length) {
        if (arr[n] == target) {
            return true
        } else {
            return false
        }
    } else {
        return false
    }
}
player.onChat("cell", function (num1) {
    player.say(grid[num1])
})
function scaleVector (scalar: number, vector: Position) {
    return pos(scalar * vector.getValue(Axis.X), scalar * vector.getValue(Axis.Y), scalar * vector.getValue(Axis.Z))
}
function playClearingAnimation () {
    for (let index = 0; index < 5; index++) {
        blocks.clone(
        ANIMATING_ANCHOR,
        positions.add(
        ANIMATING_ANCHOR,
        pos(BOARD_WIDTH - 1, 0, BOARD_HEIGHT - 1)
        ),
        positions.add(
        BOARD_ORIGIN,
        pos(1, 0, 1)
        ),
        CloneMask.Replace,
        CloneMode.Normal
        )
        blocks.clone(
        DUPE_ANCHOR,
        positions.add(
        DUPE_ANCHOR,
        pos(BOARD_WIDTH - 1, 0, BOARD_HEIGHT - 1)
        ),
        positions.add(
        BOARD_ORIGIN,
        pos(1, 0, 1)
        ),
        CloneMask.Replace,
        CloneMode.Normal
        )
    }
    blocks.clone(
    CLEARED_ANCHOR,
    positions.add(
    CLEARED_ANCHOR,
    pos(BOARD_WIDTH - 1, 0, BOARD_HEIGHT - 1)
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(1, 0, 1)
    ),
    CloneMask.Replace,
    CloneMode.Normal
    )
}
function indexToRelativeXYZ (index: number) {
    return pos(index % GRID_WIDTH - 1, 0, Math.idiv(index, GRID_WIDTH))
}
function addNextRow () {
    safeToSwap = false
    i = 1
    for (let index = 0; index < GRID_WIDTH; index++) {
        if (grid[GRID_WIDTH * BOARD_HEIGHT - i] < EMPTY_CELL && !(gameOver)) {
            gameOver = true
            gameplay.title(mobs.target(NEAREST_PLAYER), "Game over!", "")
        }
        i += 1
    }
    if (!(gameOver)) {
        for (let index = 0; index < GRID_WIDTH; index++) {
            grid.unshift(newRow.pop())
            grid.pop()
        }
        newRow = pushNextRow()
    }
    indicesToCheck = []
    i = 1
    for (let index = 0; index < BOARD_WIDTH; index++) {
        indicesToCheck.push(i)
        i += 1
    }
    safeToSwap = true
    collapseBoard()
}
function threeInARow (n: number, arr: any[], target: number) {
    verticalGoal = true
    horizontalGoal = true
    j = 1
    for (let index = 0; index < GOAL - 1; index++) {
        if (!(isValidIndexAndValueEqualTo(n - j, arr, target))) {
            horizontalGoal = false
        }
        if (!(isValidIndexAndValueEqualTo(n - GRID_WIDTH * j, arr, target))) {
            verticalGoal = false
        }
        j += 1
    }
    return horizontalGoal || verticalGoal
}
player.onItemInteracted(DIAMOND_SWORD, function () {
    player.runChatCommand("run")
})
function refreshBoard () {
    blocks.clone(
    DUPE_ANCHOR,
    positions.add(
    DUPE_ANCHOR,
    pos(BOARD_WIDTH - 1, 0, BOARD_HEIGHT - 1)
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(1, 0, 1)
    ),
    CloneMask.Replace,
    CloneMode.Normal
    )
    duplicateBoard()
}
player.onChat("row", function (num1) {
    row = ""
    i = 0
    for (let index = 0; index < GRID_WIDTH; index++) {
        row = "" + grid[num1 * GRID_WIDTH + i] + row
        i += 1
    }
    player.say(row)
})
player.onChat("push", function () {
    if (safeToPush) {
        addNextRow()
    }
})
function newRandomRow () {
    randomRow = [WALL_CELL]
    i = 0
    for (let index = 0; index < BOARD_WIDTH; index++) {
        randomIconIndex = randint(0, N_ICONS - 1)
        if (i >= GOAL - 1) {
            horizontalGoal = true
            while (horizontalGoal) {
                j = 1
                for (let index = 0; index < GOAL - 1; index++) {
                    if (randomIconIndex == randomRow[i - j]) {
                        randomIconIndex = randint(0, N_ICONS - 1)
                    } else {
                        horizontalGoal = false
                    }
                    j += 1
                }
            }
        }
        randomRow.push(randomIconIndex)
        blocks.place(NORMAL_ICONS[randomIconIndex], positions.add(
        BOARD_ORIGIN,
        pos(i + 1, 0, 0)
        ))
        i += 1
    }
    randomRow.push(WALL_CELL)
    return randomRow
}
function sameXYZ (p: Position, q: Position) {
    if (p.getValue(Axis.X) == q.getValue(Axis.X)) {
        if (p.getValue(Axis.Y) == q.getValue(Axis.Y)) {
            if (p.getValue(Axis.Z) == q.getValue(Axis.Z)) {
                return true
            }
        }
    }
    return false
}
player.onTravelled(WALK, function () {
    if (!(gameOver)) {
        myXYZ = player.position()
        if (myXYZ.getValue(Axis.Y) == BOARD_ORIGIN.getValue(Axis.Y) + 1) {
            blocks.fill(
            AIR,
            positions.add(
            BOARD_ORIGIN,
            pos(1, 1, 1)
            ),
            positions.add(
            BOARD_ORIGIN,
            pos(BOARD_WIDTH, 1, BOARD_HEIGHT)
            ),
            FillOperation.Replace
            )
            if (myXYZ.getValue(Axis.X) <= BOARD_ORIGIN.getValue(Axis.X) + 1) {
                myXYZ = positions.add(
                myXYZ,
                pos(1, 0, 0)
                )
            }
            blocks.fill(
            CURSOR_MATERIAL,
            myXYZ,
            positions.add(
            myXYZ,
            pos(-1, 0, 0)
            ),
            FillOperation.Replace
            )
        }
    }
})
player.onChat("run", function () {
    player.execute(
    "clear"
    )
    gameplay.setGameMode(
    ADVENTURE,
    mobs.target(NEAREST_PLAYER)
    )
    ORIGIN = positions.add(
    player.position(),
    pos(0, -1, 0)
    )
    player.teleport(pos(0, 3, 0))
    BOARD_ORIGIN = positions.add(
    ORIGIN,
    pos(Math.idiv(BOARD_WIDTH, -2), Y_DISTANCE, Math.idiv(BOARD_WIDTH, -2))
    )
    DUPE_ANCHOR = positions.add(
    BOARD_ORIGIN,
    pos(1, DUPE_LAYER_OFFSET, 1)
    )
    ANIMATING_ANCHOR = positions.add(
    DUPE_ANCHOR,
    pos(0, -1, 0)
    )
    CLEARED_ANCHOR = positions.add(
    ANIMATING_ANCHOR,
    pos(0, -1, 0)
    )
    SWAP_ANCHOR = positions.add(
    BOARD_ORIGIN,
    pos(0, -1, -1)
    )
    grid = initialSetup()
    newRow = newRandomRow()
    gameOver = false
    safeToSwap = true
    safeToPush = true
    mobs.give(
    mobs.target(NEAREST_PLAYER),
    SNOWBALL,
    1
    )
    while (!(gameOver)) {
        for (let index = 0; index < 10; index++) {
            loops.pause(1000)
        }
        player.execute(
        "clear"
        )
        loops.pause(1000)
        while (!(safeToPush)) {
            loops.pause(20)
        }
        addNextRow()
        mobs.give(
        mobs.target(NEAREST_PLAYER),
        SNOWBALL,
        1
        )
    }
    player.runChatCommand("end")
})
player.onChat("swap", function () {
    if (safeToSwap) {
        safeToPush = false
        leftCursor = xyzToIndex(myXYZ)
        rightCursor = xyzToIndex(myXYZ) - 1
        if (grid[leftCursor] != grid[rightCursor]) {
            grid = switchBlocks(myXYZ, grid)
            duplicateBoard()
            if (grid[leftCursor] == EMPTY_CELL || grid[rightCursor] == EMPTY_CELL) {
                if (grid[leftCursor] == EMPTY_CELL) {
                    emptyBlockIndex = leftCursor
                    nonEmptyBlockIndex = rightCursor
                } else {
                    emptyBlockIndex = rightCursor
                    nonEmptyBlockIndex = leftCursor
                }
                blocks.place(EMPTY_MATERIAL, positions.add(
                DUPE_ANCHOR,
                indexToRelativeXYZ(nonEmptyBlockIndex)
                ))
                while (isValidIndexAndValueEqualTo(nonEmptyBlockIndex - GRID_WIDTH, grid, EMPTY_CELL)) {
                    grid[nonEmptyBlockIndex - GRID_WIDTH] = grid[nonEmptyBlockIndex]
                    grid[nonEmptyBlockIndex] = EMPTY_CELL
                    nonEmptyBlockIndex += GRID_WIDTH * -1
                }
                blocks.place(NORMAL_ICONS[grid[nonEmptyBlockIndex]], positions.add(
                DUPE_ANCHOR,
                indexToRelativeXYZ(nonEmptyBlockIndex)
                ))
                indicesToCheck = [nonEmptyBlockIndex]
                if (emptyBlockIndex < GRID_WIDTH * (BOARD_HEIGHT - 1)) {
                    if (grid[emptyBlockIndex + GRID_WIDTH] < EMPTY_CELL) {
                        blocks.clone(
                        positions.add(
                        positions.add(
                        BOARD_ORIGIN,
                        pos(1, 0, 1)
                        ),
                        indexToRelativeXYZ(emptyBlockIndex + GRID_WIDTH)
                        ),
                        positions.add(
                        positions.add(
                        BOARD_ORIGIN,
                        pos(1, 0, 1)
                        ),
                        indexToRelativeXYZ(GRID_WIDTH * (BOARD_HEIGHT - 1) + emptyBlockIndex % GRID_WIDTH)
                        ),
                        positions.add(
                        DUPE_ANCHOR,
                        indexToRelativeXYZ(emptyBlockIndex)
                        ),
                        CloneMask.Replace,
                        CloneMode.Normal
                        )
                        blocks.clone(
                        positions.add(
                        positions.add(
                        BOARD_ORIGIN,
                        pos(1, 0, 1)
                        ),
                        indexToRelativeXYZ(emptyBlockIndex)
                        ),
                        positions.add(
                        positions.add(
                        BOARD_ORIGIN,
                        pos(1, 0, 1)
                        ),
                        indexToRelativeXYZ(emptyBlockIndex)
                        ),
                        positions.add(
                        DUPE_ANCHOR,
                        indexToRelativeXYZ(GRID_WIDTH * (BOARD_HEIGHT - 1) + emptyBlockIndex % GRID_WIDTH)
                        ),
                        CloneMask.Replace,
                        CloneMode.Normal
                        )
                        while (emptyBlockIndex < GRID_WIDTH * (BOARD_HEIGHT - 1)) {
                            grid[emptyBlockIndex] = grid[emptyBlockIndex + GRID_WIDTH]
                            if (grid[emptyBlockIndex] < EMPTY_CELL) {
                                indicesToCheck.push(emptyBlockIndex)
                            }
                            emptyBlockIndex += GRID_WIDTH
                        }
                        grid[emptyBlockIndex] = EMPTY_CELL
                    }
                }
                refreshBoard()
            } else {
                indicesToCheck = [leftCursor, rightCursor]
            }
            collapseBoard()
        }
        safeToPush = true
        mobs.give(
        mobs.target(NEAREST_PLAYER),
        SNOWBALL,
        1
        )
    }
})
function xyzToIndex (xyz: Position) {
    relativeX = xyz.getValue(Axis.X) - BOARD_ORIGIN.getValue(Axis.X)
    relativeZ = xyz.getValue(Axis.Z) - BOARD_ORIGIN.getValue(Axis.Z)
    return (relativeZ - 1) * GRID_WIDTH + relativeX
}
player.onChat("end", function () {
    gameOver = true
    gameplay.setGameMode(
    CREATIVE,
    mobs.target(NEAREST_PLAYER)
    )
    cleanUp()
    player.execute(
    "clear"
    )
    mobs.give(
    mobs.target(NEAREST_PLAYER),
    DIAMOND_SWORD,
    1
    )
})
function switchBlocks (pos2: Position, gridState: number[]) {
    tempGrid = gridState
    temp = tempGrid[xyzToIndex(pos2)]
    tempGrid[xyzToIndex(pos2)] = tempGrid[xyzToIndex(pos2) - 1]
    tempGrid[xyzToIndex(pos2) - 1] = temp
    i = 0
    for (let index = 0; index < 2; index++) {
        blocks.clone(
        positions.add(
        pos2,
        pos(i - 1, -1, 0)
        ),
        positions.add(
        pos2,
        pos(i - 1, -1, 0)
        ),
        positions.add(
        SWAP_ANCHOR,
        pos(1 - i, 0, 0)
        ),
        CloneMask.Replace,
        CloneMode.Normal
        )
        i += 1
    }
    blocks.clone(
    SWAP_ANCHOR,
    positions.add(
    SWAP_ANCHOR,
    pos(1, 0, 0)
    ),
    positions.add(
    pos2,
    pos(-1, -1, 0)
    ),
    CloneMask.Replace,
    CloneMode.Move
    )
    return tempGrid
}
function duplicateBoard () {
    i = 0
    for (let index = 0; index < 3; index++) {
        blocks.clone(
        positions.add(
        BOARD_ORIGIN,
        pos(1, 0, 1)
        ),
        positions.add(
        BOARD_ORIGIN,
        pos(BOARD_WIDTH, 0, BOARD_HEIGHT)
        ),
        positions.add(
        DUPE_ANCHOR,
        pos(0, i, 0)
        ),
        CloneMask.Replace,
        CloneMode.Normal
        )
        i += -1
    }
}
player.onTravelled(SNEAK, function () {
    if (!(gameOver)) {
        myXYZ = player.position()
        if (myXYZ.getValue(Axis.Y) == BOARD_ORIGIN.getValue(Axis.Y) + 1) {
            blocks.fill(
            AIR,
            positions.add(
            BOARD_ORIGIN,
            pos(1, 1, 1)
            ),
            positions.add(
            BOARD_ORIGIN,
            pos(BOARD_WIDTH, 1, BOARD_HEIGHT)
            ),
            FillOperation.Replace
            )
            if (myXYZ.getValue(Axis.X) <= BOARD_ORIGIN.getValue(Axis.X) + 1) {
                myXYZ = positions.add(
                myXYZ,
                pos(1, 0, 0)
                )
            }
            blocks.fill(
            CURSOR_MATERIAL,
            myXYZ,
            positions.add(
            myXYZ,
            pos(-1, 0, 0)
            ),
            FillOperation.Replace
            )
        }
    }
})
function collapseBoard () {
    deleteTheseIndices = identifyBlocksToClear(grid, indicesToCheck)
    while (deleteTheseIndices.length > 0) {
        for (let value of deleteTheseIndices) {
            grid[value] = EMPTY_CELL
        }
        indicesToCheck = []
        deleteTheseIndices = []
        i = 1
        for (let index = 0; index < BOARD_WIDTH; index++) {
            originalColumn = []
            collapsedColumn = []
            collapseToThisIndex = 0
            lowestChangedIndexSet = false
            lowestChangedIndex = -1
            j = 0
            for (let index = 0; index < BOARD_HEIGHT; index++) {
                if (j == 0) {
                    previousCellValue = 0
                } else {
                    previousCellValue = grid[(j - 1) * GRID_WIDTH + i]
                }
                cellValue = grid[j * GRID_WIDTH + i]
                originalColumn.push(cellValue)
                if (cellValue < EMPTY_CELL) {
                    collapsedColumn.push(cellValue)
                    if (previousCellValue == EMPTY_CELL) {
                        blocks.clone(
                        positions.add(
                        BOARD_ORIGIN,
                        pos(i, 0, j + 1)
                        ),
                        positions.add(
                        BOARD_ORIGIN,
                        pos(i, 0, BOARD_HEIGHT)
                        ),
                        positions.add(
                        DUPE_ANCHOR,
                        pos(i - 1, 0, collapseToThisIndex)
                        ),
                        CloneMask.Replace,
                        CloneMode.Normal
                        )
                        if (!(lowestChangedIndexSet)) {
                            lowestChangedIndex = collapseToThisIndex
                            lowestChangedIndexSet = true
                        }
                    }
                    collapseToThisIndex += 1
                }
                j += 1
            }
            while (collapsedColumn.length < BOARD_HEIGHT) {
                collapsedColumn.push(EMPTY_CELL)
            }
            j = 0
            for (let value of collapsedColumn) {
                grid[j * GRID_WIDTH + i] = value
                j += 1
            }
            if (lowestChangedIndex >= 0) {
                j = lowestChangedIndex
                while (collapsedColumn[j] < EMPTY_CELL) {
                    indicesToCheck.push(j * GRID_WIDTH + i)
                    j += 1
                }
            }
            emptyIndex = collapsedColumn.indexOf(EMPTY_CELL)
            if (emptyIndex >= 0) {
                blocks.fill(
                EMPTY_MATERIAL,
                positions.add(
                DUPE_ANCHOR,
                pos(i - 1, 0, emptyIndex)
                ),
                positions.add(
                DUPE_ANCHOR,
                pos(i - 1, 0, BOARD_HEIGHT - 1)
                ),
                FillOperation.Replace
                )
            }
            i += 1
        }
        refreshBoard()
        deleteTheseIndices = identifyBlocksToClear(grid, indicesToCheck)
    }
}
function delay (ticks: number) {
    for (let index = 0; index < ticks; index++) {
        if (blocks.testForBlock(GRASS, pos(0, 0, 0))) {
        	
        }
    }
}
function initialSetup () {
    newGrid = []
    blocks.fill(
    EDGE_MATERIAL,
    positions.add(
    BOARD_ORIGIN,
    pos(0, CURSOR_LAYER_OFFSET, -1)
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(BOARD_WIDTH + 1, 0, BOARD_HEIGHT + 1)
    ),
    FillOperation.Replace
    )
    blocks.fill(
    EMPTY_MATERIAL,
    positions.add(
    BOARD_ORIGIN,
    pos(1, 0, 1)
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(BOARD_WIDTH, 0, BOARD_HEIGHT)
    ),
    FillOperation.Replace
    )
    blocks.fill(
    BACKGROUND_MATERIAL,
    positions.add(
    BOARD_ORIGIN,
    pos(0, BACKGROUND_OFFSET, 0)
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(GRID_WIDTH - 1, BACKGROUND_OFFSET, BOARD_HEIGHT + 1)
    ),
    FillOperation.Replace
    )
    i = 0
    for (let index = 0; index < GRID_WIDTH * BOARD_HEIGHT; index++) {
        if (i % GRID_WIDTH == 0 || i % GRID_WIDTH == GRID_WIDTH - 1) {
            newGrid.push(WALL_CELL)
        } else if (i < Math.idiv(GRID_WIDTH * BOARD_HEIGHT, 2)) {
            randomIconIndex = randint(0, N_ICONS - 1)
            while (threeInARow(i, newGrid, randomIconIndex)) {
                randomIconIndex = randint(0, N_ICONS - 1)
            }
            newGrid.push(randomIconIndex)
            blocks.place(NORMAL_ICONS[randomIconIndex], positions.add(
            BOARD_ORIGIN,
            pos(i % GRID_WIDTH, 0, Math.idiv(i, GRID_WIDTH) + 1)
            ))
        } else {
            newGrid.push(EMPTY_CELL)
        }
        i += 1
    }
    blocks.fill(
    PREVIEW_MATERIAL,
    positions.add(
    BOARD_ORIGIN,
    pos(1, CURSOR_LAYER_OFFSET, 0)
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(BOARD_WIDTH, CURSOR_LAYER_OFFSET, 0)
    ),
    FillOperation.Replace
    )
    blocks.fill(
    AIR,
    positions.add(
    BOARD_ORIGIN,
    pos(1, CURSOR_LAYER_OFFSET, 1)
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(BOARD_WIDTH, CURSOR_LAYER_OFFSET + 2, BOARD_HEIGHT)
    ),
    FillOperation.Replace
    )
    blocks.place(ORANGE_WOOL, BOARD_ORIGIN)
    duplicateBoard()
    return newGrid
}
function identifyBlocksToClear (gridState: number[], changedIndices: number[]) {
    indicesToCollapse = []
    for (let value4 of changedIndices) {
        for (let value22 of AXES) {
            i = 0
            for (let index = 0; index < GOAL; index++) {
                rootIndex = value4 + i * (value22.getValue(Axis.X) + GRID_WIDTH * value22.getValue(Axis.Z))
                if (rootIndex >= 0 && rootIndex < GRID_WIDTH * BOARD_HEIGHT) {
                    j = 0
                    matchingIndices = []
                    for (let index = 0; index < GOAL; index++) {
                        comparisonIndex = rootIndex + j * (value22.getValue(Axis.X) + GRID_WIDTH * value22.getValue(Axis.Z))
                        if (comparisonIndex >= 0 && comparisonIndex < GRID_WIDTH * BOARD_HEIGHT) {
                            if (gridState[rootIndex] == gridState[comparisonIndex]) {
                                matchingIndices.push(comparisonIndex)
                            }
                        }
                        j += 1
                    }
                    if (matchingIndices.length >= GOAL) {
                        for (let value of matchingIndices) {
                            if (indicesToCollapse.indexOf(value) < 0) {
                                indicesToCollapse.push(value)
                            }
                        }
                        blocks.fill(
                        NORMAL_ICONS[gridState[value4]],
                        positions.add(
                        DUPE_ANCHOR,
                        indexToRelativeXYZ(rootIndex)
                        ),
                        positions.add(
                        DUPE_ANCHOR,
                        indexToRelativeXYZ(comparisonIndex)
                        ),
                        FillOperation.Replace
                        )
                        blocks.fill(
                        ANIMATING_ICONS[gridState[value4]],
                        positions.add(
                        ANIMATING_ANCHOR,
                        indexToRelativeXYZ(rootIndex)
                        ),
                        positions.add(
                        ANIMATING_ANCHOR,
                        indexToRelativeXYZ(comparisonIndex)
                        ),
                        FillOperation.Replace
                        )
                        blocks.fill(
                        CLEARED_ICONS[gridState[value4]],
                        positions.add(
                        CLEARED_ANCHOR,
                        indexToRelativeXYZ(rootIndex)
                        ),
                        positions.add(
                        CLEARED_ANCHOR,
                        indexToRelativeXYZ(comparisonIndex)
                        ),
                        FillOperation.Replace
                        )
                    }
                }
                i += -1
            }
        }
    }
    if (indicesToCollapse.length > 0) {
        playClearingAnimation()
    }
    sortedIndices = []
    for (let value of indicesToCollapse) {
        if (sortedIndices.length == 0) {
            sortedIndices.push(value)
        } else if (sortedIndices.length == 1) {
            if (value > sortedIndices[0]) {
                sortedIndices.push(value)
            } else {
                sortedIndices.unshift(value)
            }
        } else {
            i = 0
            for (let index = 0; index < sortedIndices.length; index++) {
                if (value < sortedIndices[i] && sortedIndices.indexOf(value) < 0) {
                    sortedIndices.insertAt(i, value)
                }
                i += 1
            }
            if (sortedIndices.indexOf(value) < 0) {
                sortedIndices.push(value)
            }
        }
    }
    for (let value of sortedIndices) {
        delay(1)
        blocks.place(EMPTY_MATERIAL, positions.add(
        BOARD_ORIGIN,
        positions.add(
        indexToRelativeXYZ(value),
        pos(1, 0, 1)
        )
        ))
    }
    return indicesToCollapse
}
function pushNextRow () {
    blocks.fill(
    AIR,
    positions.add(
    BOARD_ORIGIN,
    pos(1, 0, BOARD_HEIGHT)
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(BOARD_WIDTH, 0, BOARD_HEIGHT)
    ),
    FillOperation.Replace
    )
    blocks.fill(
    blocks.blockWithData(PISTON, 2),
    positions.add(
    BOARD_ORIGIN,
    pos(1, 0, -1)
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(BOARD_WIDTH, 0, -1)
    ),
    FillOperation.Replace
    )
    blocks.fill(
    REDSTONE_BLOCK,
    positions.add(
    BOARD_ORIGIN,
    pos(1, BACKGROUND_OFFSET, -1)
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(BOARD_WIDTH, BACKGROUND_OFFSET, -1)
    ),
    FillOperation.Replace
    )
    delay(2)
    blocks.fill(
    AIR,
    positions.add(
    BOARD_ORIGIN,
    pos(1, BACKGROUND_OFFSET, -1)
    ),
    positions.add(
    BOARD_ORIGIN,
    pos(BOARD_WIDTH, BACKGROUND_OFFSET, -1)
    ),
    FillOperation.Replace
    )
    delay(3)
    duplicateBoard()
    nextRow = newRandomRow()
    return nextRow
}
let nextRow: number[] = []
let sortedIndices: number[] = []
let comparisonIndex = 0
let matchingIndices: number[] = []
let rootIndex = 0
let indicesToCollapse: number[] = []
let newGrid: number[] = []
let emptyIndex = 0
let cellValue = 0
let previousCellValue = 0
let lowestChangedIndex = 0
let lowestChangedIndexSet = false
let collapseToThisIndex = 0
let collapsedColumn: number[] = []
let originalColumn: number[] = []
let deleteTheseIndices: number[] = []
let temp = 0
let tempGrid: number[] = []
let relativeZ = 0
let relativeX = 0
let nonEmptyBlockIndex = 0
let emptyBlockIndex = 0
let rightCursor = 0
let leftCursor = 0
let SWAP_ANCHOR: Position = null
let ORIGIN: Position = null
let myXYZ: Position = null
let randomIconIndex = 0
let randomRow: number[] = []
let safeToPush = false
let row = ""
let j = 0
let horizontalGoal = false
let verticalGoal = false
let indicesToCheck: number[] = []
let newRow: number[] = []
let safeToSwap = false
let CLEARED_ANCHOR: Position = null
let DUPE_ANCHOR: Position = null
let ANIMATING_ANCHOR: Position = null
let grid: number[] = []
let BOARD_ORIGIN: Position = null
let AXES: Position[] = []
let gameOver = false
let GOAL = 0
let WALL_CELL = 0
let EMPTY_CELL = 0
let DUPE_LAYER_OFFSET = 0
let BACKGROUND_OFFSET = 0
let CURSOR_LAYER_OFFSET = 0
let GRID_WIDTH = 0
let Y_DISTANCE = 0
let BOARD_HEIGHT = 0
let BOARD_WIDTH = 0
let BACKGROUND_MATERIAL = 0
let EDGE_MATERIAL = 0
let EMPTY_MATERIAL = 0
let PREVIEW_MATERIAL = 0
let CURSOR_MATERIAL = 0
let CLEARED_ICONS: number[] = []
let ANIMATING_ICONS: number[] = []
let i = 0
let NORMAL_ICONS: number[] = []
let N_ICONS = 0
N_ICONS = 6
let ANIMATING_ICONS_ID_OFFSET = 20
let CLEARED_ICONS_ID_OFFSET = 40
NORMAL_ICONS = []
i = 1
for (let index = 0; index < N_ICONS; index++) {
    NORMAL_ICONS.push(blocks.blockByName("element_" + i))
    ANIMATING_ICONS.push(blocks.blockByName("element_" + (i + ANIMATING_ICONS_ID_OFFSET)))
    CLEARED_ICONS.push(blocks.blockByName("element_" + (i + CLEARED_ICONS_ID_OFFSET)))
    i += 1
}
CURSOR_MATERIAL = blocks.blockWithData(GLOW_LICHEN, 1)
PREVIEW_MATERIAL = BLACK_STAINED_GLASS
EMPTY_MATERIAL = GLASS
EDGE_MATERIAL = SEA_LANTERN
BACKGROUND_MATERIAL = CHERRY_PLANKS
BOARD_WIDTH = 6
BOARD_HEIGHT = 12
Y_DISTANCE = -1
GRID_WIDTH = BOARD_WIDTH + 2
CURSOR_LAYER_OFFSET = 1
BACKGROUND_OFFSET = -1
DUPE_LAYER_OFFSET = BACKGROUND_OFFSET - 1
EMPTY_CELL = NORMAL_ICONS.length
WALL_CELL = EMPTY_CELL + 1
let DELETION_CELL = WALL_CELL + 1
GOAL = 3
gameOver = true
let UNIT_X = pos(1, 0, 0)
let UNIT_Z = pos(0, 0, 1)
AXES = [UNIT_X, UNIT_Z]
player.execute(
"clear"
)
mobs.give(
mobs.target(NEAREST_PLAYER),
DIAMOND_SWORD,
1
)

```
    
