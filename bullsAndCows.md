# Bulls and Cows

## Step 1

Throw a snowball to start the game.  Try to deduce the 4-block code.

Place a block under the agent to enter one part of the code.

```template
player.onChat("here", function () {
    agent.teleportToPlayer()
})
player.onItemInteracted(SNOWBALL, function () {
    player.runChatCommand("play")
})
function showHits (count: number, blockType: number) {
    for (let index = 0; index < count; index++) {
        agent.move(UP, 1)
        setBlockBelowAgent(1, blockType)
    }
}
function generateCode () {
    BLOCKS = [
    GOLD_BLOCK,
    IRON_BLOCK,
    COAL_BLOCK,
    DIAMOND_BLOCK,
    EMERALD_BLOCK,
    PURPUR_BLOCK,
    STONE,
    GLOWSTONE,
    LAPIS_LAZULI_BLOCK
    ]
    agentCode = []
    while (agentCode.length < CODE_SIZE) {
        randomBlock = BLOCKS[randint(0, BLOCKS.length - 1)]
        if (agentCode.indexOf(randomBlock) < 0) {
            agentCode.push(randomBlock)
        }
    }
}
player.onChat("play", function () {
    gameplay.setGameMode(
    ADVENTURE,
    mobs.target(NEAREST_PLAYER)
    )
    generateCode()
    guessesLeft = 7
    gameOver = false
    agent.move(UP, 1)
    player.say("You have " + guessesLeft + " guesses to crack the code")
    while (!(gameOver)) {
        player.execute(
        "clear"
        )
        for (let value of BLOCKS) {
            mobs.give(
            mobs.target(NEAREST_PLAYER),
            value,
            1
            )
        }
        perfectMatches = 0
        colorMatches = 0
        for (let value of agentCode) {
            setBlockBelowAgent(2, ALLOW)
            while (!(agent.detect(AgentDetection.Block, DOWN))) {
                loops.pause(1000)
            }
            setBlockBelowAgent(2, GROUND)
            currentBlock = agent.inspect(AgentInspection.Block, DOWN)
            if (currentBlock == value) {
                perfectMatches += 1
                if (perfectMatches == CODE_SIZE) {
                    gameOver = true
                    player.say("You cracked the code!")
                    player.say("You win!")
                }
            } else if (agentCode.indexOf(currentBlock) >= 0) {
                colorMatches += 1
            }
            agent.move(LEFT, 1)
        }
        showHits(colorMatches, COLOR_MATCH)
        showHits(perfectMatches, PERFECT_MATCH)
        agent.move(RIGHT, CODE_SIZE)
        agent.move(DOWN, perfectMatches + colorMatches)
        agent.move(FORWARD, 1)
        if (!(gameOver)) {
            guessesLeft += -1
        }
        if (guessesLeft == 0) {
            gameOver = true
            player.say("You ran out of guesses")
            player.say("The agent wins")
        }
    }
    agent.move(DOWN, 1)
    for (let value of agentCode) {
        setBlockBelowAgent(1, value)
        agent.move(LEFT, 1)
    }
    agent.move(RIGHT, CODE_SIZE)
    agent.move(FORWARD, 2)
    player.execute(
    "clear"
    )
    gameplay.setGameMode(
    CREATIVE,
    mobs.target(NEAREST_PLAYER)
    )
})
function setBlockBelowAgent (distance: number, blockType: number) {
    blocks.place(blockType, positions.add(
    agent.getPosition(),
    pos(0, distance * -1, 0)
    ))
}
let currentBlock = 0
let colorMatches = 0
let perfectMatches = 0
let gameOver = false
let guessesLeft = 0
let randomBlock = 0
let agentCode: number[] = []
let BLOCKS: number[] = []
let CODE_SIZE = 0
let ALLOW = 0
let GROUND = 0
let COLOR_MATCH = 0
let PERFECT_MATCH = 0
PERFECT_MATCH = REDSTONE_BLOCK
COLOR_MATCH = SEA_LANTERN
GROUND = agent.inspect(AgentInspection.Block, DOWN)
ALLOW = blocks.blockByName("allow")
CODE_SIZE = 4
mobs.give(
mobs.target(NEAREST_PLAYER),
SNOWBALL,
1
)
```
    
