# Simon

## Step 1

Type "play" in chat to start a game.  Recreate the ever-growing block sequence shown by the agent.

Place blocks underneath the agent.  You only have a short amount of time to place each block.

```template
player.onChat("here", function () {
    agent.teleportToPlayer()
})
function givePlayerBlocks () {
    for (let value of BLOCKS) {
        mobs.give(
        mobs.target(NEAREST_PLAYER),
        value,
        1
        )
    }
}
function showCorrectBlock (b: number) {
    agent.setItem(b, 1, 1)
    agent.setSlot(1)
    agent.place(UP)
    wait(LONG)
    agent.destroy(UP)
}
function showBlock (b: number) {
    agent.setSlot(b)
    agent.place(DOWN)
    wait(SHORT)
    agent.destroy(DOWN)
}
function beginPlayerTurn () {
    player.say("your turn...")
    wait(MEDIUM)
    agent.move(UP, 1)
    player.say("go!")
}
player.onChat("play", function () {
    GROUND = agent.inspect(AgentInspection.Block, DOWN)
    gameplay.setGameMode(
    ADVENTURE,
    mobs.target(NEAREST_PLAYER)
    )
    player.execute(
    "clear"
    )
    score = 0
    gameOver = false
    list = []
    blocks.place(blocks.blockByName("allow"), positions.add(
    agent.getPosition(),
    pos(0, -1, 0)
    ))
    while (!(gameOver)) {
        agent.move(UP, 1)
        for (let index = 0; index < INCREMENT; index++) {
            list.push(randint(1, N_BLOCKS))
        }
        length = list.length
        for (let value of list) {
            giveAgentBlocks()
            showBlock(value)
        }
        agent.move(DOWN, 1)
        givePlayerBlocks()
        beginPlayerTurn()
        ii = 0
        while (!(gameOver) && ii < length) {
            wait(MEDIUM)
            targetBlock = BLOCKS[list[ii] - 1]
            playerBlock = agent.inspect(AgentInspection.Block, DOWN)
            if (playerBlock == targetBlock) {
                mobs.give(
                mobs.target(NEAREST_PLAYER),
                targetBlock,
                1
                )
                ii += 1
                if (score < ii) {
                    score = ii
                }
            } else {
                if (playerBlock == AIR_BLOCK) {
                    player.say("Too slow!")
                } else {
                    player.say("Wrong block!")
                }
                gameOver = true
            }
            agent.destroy(DOWN)
        }
        player.execute(
        "clear"
        )
        agent.move(DOWN, 1)
        if (gameOver) {
            player.say("Game over. Your score: " + score)
            player.say("the correct block was...")
            showCorrectBlock(targetBlock)
        } else {
            player.say("nicely done.")
            wait(LONG)
        }
    }
    blocks.place(GROUND, positions.add(
    agent.getPosition(),
    pos(0, -1, 0)
    ))
})
function wait (ticks: number) {
    for (let index = 0; index < ticks; index++) {
        if (blocks.testForBlock(GRASS, pos(0, 0, 0))) {
        	
        }
    }
}
function giveAgentBlocks () {
    for (let value of BLOCKS) {
        agent.setItem(value, 1, BLOCKS.indexOf(value) + 1)
    }
}
let playerBlock = 0
let targetBlock = 0
let ii = 0
let length = 0
let list: number[] = []
let gameOver = false
let score = 0
let GROUND = 0
let SHORT = 0
let MEDIUM = 0
let LONG = 0
let N_BLOCKS = 0
let BLOCKS: number[] = []
let AIR_BLOCK = 0
let INCREMENT = 0
INCREMENT = 3
AIR_BLOCK = AIR
BLOCKS = [
GOLD_BLOCK,
IRON_BLOCK,
DIAMOND_BLOCK,
EMERALD_BLOCK
]
N_BLOCKS = BLOCKS.length
LONG = 40
MEDIUM = 20
SHORT = 10

```
    
