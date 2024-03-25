# Rock, Paper, Scissors

## Step 1

Type "play" in chat to start a match.  First player to get three points wins.

Diamond = Rock / Gold = Paper / Emerald = Scissors

```template
function agentThrow () {
    agent.setSlot(randint(1, 3))
    agent.place(UP)
    return agent.inspect(AgentInspection.Block, UP)
}
function preparePlayer () {
    player.execute(
    "clear"
    )
    mobs.give(
    mobs.target(NEAREST_PLAYER),
    ROCK,
    1
    )
    mobs.give(
    mobs.target(NEAREST_PLAYER),
    PAPER2,
    1
    )
    mobs.give(
    mobs.target(NEAREST_PLAYER),
    SCISSORS,
    1
    )
}
player.onChat("play", function () {
    gameplay.setGameMode(
    ADVENTURE,
    mobs.target(NEAREST_PLAYER)
    )
    ground = agent.inspect(AgentInspection.Block, DOWN)
    blocks.place(blocks.blockByName("allow"), positions.add(
    agent.getPosition(),
    pos(0, -1, 0)
    ))
    playerScore = 0
    agentScore = 0
    agent.move(UP, 1)
    while (playerScore < GOAL && agentScore < GOAL) {
        prepareAgent()
        preparePlayer()
        while (!(agent.detect(AgentDetection.Block, DOWN))) {
        	
        }
        playerChoice = agent.inspect(AgentInspection.Block, DOWN)
        agentChoice = agentThrow()
        player.execute(
        "clear"
        )
        if (playerChoice == agentChoice) {
            player.say("It's a draw")
        } else if (playerChoice == ROCK) {
            if (agentChoice == SCISSORS) {
                player.say("You got a point!")
                playerScore += 1
            } else {
                player.say("The agent got a point.")
                agentScore += 1
            }
        } else if (playerChoice == PAPER2) {
            if (agentChoice == ROCK) {
                player.say("You got a point!")
                playerScore += 1
            } else {
                player.say("The agent got a point.")
                agentScore += 1
            }
        } else {
            if (agentChoice == PAPER2) {
                player.say("You got a point!")
                playerScore += 1
            } else {
                player.say("The agent got a point.")
                agentScore += 1
            }
        }
        loops.pause(2000)
        agent.destroy(DOWN)
        agent.destroy(UP)
    }
    agent.move(DOWN, 1)
    blocks.place(ground, positions.add(
    agent.getPosition(),
    pos(0, -1, 0)
    ))
    if (playerScore == GOAL) {
        player.say("You won the match!")
    } else {
        player.say("The agent won the match.")
    }
    gameplay.setGameMode(
    CREATIVE,
    mobs.target(NEAREST_PLAYER)
    )
})
function prepareAgent () {
    agent.setItem(ROCK, 1, 1)
    agent.setItem(PAPER2, 1, 2)
    agent.setItem(SCISSORS, 1, 3)
}
let agentChoice = 0
let playerChoice = 0
let agentScore = 0
let playerScore = 0
let ground = 0
let GOAL = 0
let SCISSORS = 0
let PAPER2 = 0
let ROCK = 0
ROCK = DIAMOND_BLOCK
PAPER2 = GOLD_BLOCK
SCISSORS = EMERALD_BLOCK
GOAL = 3

```
    
