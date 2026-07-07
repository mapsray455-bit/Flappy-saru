<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>💖 Flappy Saru 💖</title>

<style>

*{
    margin:0;
    padding:0;
    box-sizing:border-box;
}

body{
    overflow:hidden;
    font-family:'Segoe UI',sans-serif;
    background:linear-gradient(
        135deg,
        #ff9a9e,
        #fad0c4,
        #ffd6e0
    );
}

#game{
    position:relative;
    width:100vw;
    height:100vh;
    overflow:hidden;
}

#bird{
    position:absolute;
    left:120px;
    top:250px;

    background:#ff4d6d;
    color:white;

    padding:10px 18px;

    border-radius:30px;

    font-weight:bold;
    font-size:28px;

    box-shadow:
    0 5px 15px rgba(0,0,0,.2);

    z-index:10;
    user-select:none;
}

.pipe{
    position:absolute;

    width:80px;

    background:
    linear-gradient(
        #ff6b9d,
        #ff3f7f
    );

    border:4px solid #ff2d6f;

    border-radius:25px;

    box-shadow:
    0 0 15px rgba(0,0,0,.15);
}

.silverPipe{
    background:
    linear-gradient(
        #eeeeee,
        #9f9f9f
    ) !important;

    border-color:#777 !important;
}

.goldPipe{
    background:
    linear-gradient(
        gold,
        #FFD700
    ) !important;

    border-color:#d4a400 !important;
}

#score{
    position:absolute;
    top:15px;
    left:20px;

    color:white;

    font-size:30px;
    font-weight:bold;

    text-shadow:
    2px 2px 6px black;

    z-index:100;
}

#highScore{
    position:absolute;
    top:15px;
    right:20px;

    color:white;

    font-size:30px;
    font-weight:bold;

    text-shadow:
    2px 2px 6px black;

    z-index:100;
}

.overlay{
    position:absolute;
    inset:0;

    background:
    rgba(0,0,0,.55);

    display:flex;
    justify-content:center;
    align-items:center;
    flex-direction:column;

    text-align:center;

    color:white;

    z-index:500;
}

button{
    margin-top:20px;

    padding:12px 28px;

    border:none;

    border-radius:30px;

    cursor:pointer;

    font-size:18px;
    font-weight:bold;

    background:#ff4d6d;
    color:white;
}

button:hover{
    transform:scale(1.05);
}

.cloud{
    position:absolute;
    font-size:50px;
    opacity:.7;
    color:white;
    animation:floatCloud 20s linear infinite;
}

@keyframes floatCloud{

    from{
        transform:translateX(100vw);
    }

    to{
        transform:translateX(-150px);
    }
}

@media(max-width:600px){

    #bird{
        font-size:22px;
        padding:8px 14px;
    }

    .pipe{
        width:70px;
    }

    #score,
    #highScore{
        font-size:22px;
    }
}

</style>
</head>
<body>

<div id="game">

    <div class="cloud"
         style="top:10%;animation-duration:22s;">
         ☁️
    </div>

    <div class="cloud"
         style="top:35%;animation-duration:18s;">
         ☁️
    </div>

    <div class="cloud"
         style="top:60%;animation-duration:25s;">
         ☁️
    </div>

    <div id="score">
        Score: 0
    </div>

    <div id="highScore">
        High: 0
    </div>

    <div id="bird">
        Saru ❤️
    </div>

    <div id="startScreen"
         class="overlay">

        <h1>
            💖 Flappy Saru 💖
        </h1>

        <br>

        <p>
            Pass 100 pipes
            and then clear
            the Golden Pipe!
        </p>

        <button onclick="startGame()">
            Start Game
        </button>

    </div>

    <div id="gameOver"
         class="overlay"
         style="display:none;">

        <h1 id="endTitle">
        </h1>

        <br>

        <div id="endText">
        </div>

        <button onclick="location.reload()">
            Play Again Saru?
        </button>

    </div>
  
  <script>

const bird =
document.getElementById("bird");

const game =
document.getElementById("game");

const scoreText =
document.getElementById("score");

const highScoreText =
document.getElementById("highScore");

let birdY = 250;
let velocity = 0;

const gravity = 0.55;
const jump = -9;

let score = 0;
let gameRunning = false;

let pipes = [];

let pipeInterval;

let highScore =
parseInt(
localStorage.getItem(
"flappySaruHighScore"
)
) || 0;

highScoreText.innerHTML =
"High: " + highScore;

function flap(){

    if(gameRunning){

        velocity = jump;

    }
}

document.addEventListener(
"keydown",
(e)=>{

    if(e.code==="Space"){

        flap();

    }
});

document.addEventListener(
"click",
()=>{

    flap();

});

function createPipe(){

    const gap = 200;

    const topHeight =
    Math.random()*250 + 80;

    const bottomHeight =
    window.innerHeight
    -
    topHeight
    -
    gap;

    const topPipe =
    document.createElement("div");

    topPipe.className =
    "pipe";

    topPipe.style.height =
    topHeight + "px";

    topPipe.style.left =
    window.innerWidth + "px";

    topPipe.style.top = "0";

    const bottomPipe =
    document.createElement("div");

    bottomPipe.className =
    "pipe";

    bottomPipe.style.height =
    bottomHeight + "px";

    bottomPipe.style.left =
    window.innerWidth + "px";

    bottomPipe.style.bottom =
    "0";

    const nextPipeNumber =
    score + 1;

    if(
       nextPipeNumber ===
       highScore
       &&
       highScore > 0
    ){

        topPipe.classList.add(
        "silverPipe"
        );

        bottomPipe.classList.add(
        "silverPipe"
        );
    }

    if(score >= 100){

        topPipe.classList.add(
        "goldPipe"
        );

        bottomPipe.classList.add(
        "goldPipe"
        );

        topPipe.dataset.gold =
        "true";

        bottomPipe.dataset.gold =
        "true";
    }

    game.appendChild(topPipe);

    game.appendChild(bottomPipe);

    pipes.push({

        x:window.innerWidth,

        scored:false,

        top:topPipe,

        bottom:bottomPipe

    });
}

function startGame(){

    document
    .getElementById(
    "startScreen"
    )
    .style.display =
    "none";

    gameRunning = true;

    birdY = 250;

    velocity = 0;

    score = 0;

    scoreText.innerHTML =
    "Score: 0";

    pipeInterval =
    setInterval(
    createPipe,
    1800
    );

    requestAnimationFrame(
    update
    );
}
  function update(){

    if(!gameRunning)
        return;

    velocity += gravity;

    birdY += velocity;

    bird.style.top =
    birdY + "px";

    if(
        birdY < 0
        ||
        birdY >
        window.innerHeight - 80
    ){
        gameOver();
        return;
    }

    const birdRect =
    bird.getBoundingClientRect();

    const hitBox = {

        left:
        birdRect.left + 10,

        right:
        birdRect.right - 10,

        top:
        birdRect.top + 10,

        bottom:
        birdRect.bottom - 10

    };

    pipes.forEach(pipe=>{

        pipe.x -= 4;

        pipe.top.style.left =
        pipe.x + "px";

        pipe.bottom.style.left =
        pipe.x + "px";

        if(
            !pipe.scored
            &&
            pipe.x < 120
        ){

            pipe.scored = true;

            score++;

            scoreText.innerHTML =
            "Score: " + score;

            if(
                score >
                highScore
            ){

                highScore =
                score;

                localStorage.setItem(
                "flappySaruHighScore",
                highScore
                );

                highScoreText.innerHTML =
                "High: " +
                highScore;
            }

            if(
                score === 101
                &&
                pipe.top.dataset.gold
                === "true"
            ){

                winGame();
            }
        }

        const topRect =
        pipe.top
        .getBoundingClientRect();

        const bottomRect =
        pipe.bottom
        .getBoundingClientRect();

        const collision =

        hitBox.left <
        topRect.right

        &&

        hitBox.right >
        topRect.left

        &&

        (
            hitBox.top <
            topRect.bottom

            ||

            hitBox.bottom >
            bottomRect.top
        );

        if(collision){

            gameOver();
        }

    });

    requestAnimationFrame(
    update
    );
}

function gameOver(){

    gameRunning = false;

    clearInterval(
    pipeInterval
    );

    document
    .getElementById(
    "gameOver"
    )
    .style.display =
    "flex";

    document
    .getElementById(
    "endTitle"
    )
    .innerHTML =
    "💔 Saru Crashed";

    document
    .getElementById(
    "endText"
    )
    .innerHTML =

    "🏆 Score: "
    + score +

    "<br><br>⭐ High Score: "
    + highScore +

    "<br><br>Play Again?";
}

function winGame(){

    gameRunning = false;

    clearInterval(
    pipeInterval
    );

    document
    .getElementById(
    "gameOver"
    )
    .style.display =
    "flex";

    document
    .getElementById(
    "endTitle"
    )
    .innerHTML =

    "🏆 Congratulations Saru!";

    document
    .getElementById(
    "endText"
    )
    .innerHTML =

    "💖 You passed<br>" +

    "100 pipes and the<br>" +

    "Golden Pipe!<br><br>" +

    "Wanna Play Again Saru?";
}

</script>

</body>
</html>
