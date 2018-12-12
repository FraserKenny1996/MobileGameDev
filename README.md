# MobileGameDev
CourseWorkSubmission

Mobile Game Development 1 Course Work 2018
Fraser Kenny S1431557

To briefly explain each scrip is copied out onto the section before its respective explanation, only the key/important parts of the code are explained


<!DOCTYPE html>
<html lang="en">
   <head>
       <meta charset="utf-8">
       <title>Snake</title>
       <link rel="stylesheet" href="web.css">
       <script>require=()=>({}); module={}; module.exports=()=>({})</script>
   </head>
   <body>
   <audio src="audio/nani.mp3" id="my_audio" loop="loop" autoplay="autoplay"></audio>
   <script type="text/javascript">
   window.onload=function(){
     document.getElementById("my_audio").play();
   }
   </script>
   <div id = "startScreen">
       <div>
           <h1>Fraser's Bad Snake Game</h1>
       </div>

       <div>
           <p>click the button to play the game, clicking it multiple times is not a good idea</p>
           <p>Use either wasd, hjkl or the arrow keys to move, To replay simply click the button again</p>
           <p>East apples to increase your score, if you hit yourself you will GameOver</p>
       </div>
       <button onclick="myFunction()">Start</button>
   </div>
       <canvas id="canvas" width="700" height="500"></canvas>
       <script src="base.js"></script>
       <script src="snake.js"></script>
       <script src="web.js"></script>
       <div id="myProgress">
           <div id="myBar">10%</div>
       </div>
   </body>
</html>

snake.HTML
Document type is set as HTML
The language is set to English
Inside the header the character set is set to UTF 8 (universal Character Set + Transformation Format 8 bit)
Set the title of the page to “Snake”
Define a preferred stylesheet
Inside the body I start with setting audio that should auto play on load (However, it doesn’t seem to work)
A division is created with name Start Screen
Division is given header “Fraser’s Bad Snake Game”
Several sections of text are displayed which explain the game and the button
Button with text start and attached to function “myFunction” and is active onCLick
Set name for the canvas to canvas, set width and height respectively 
Link the scripts that we will be using: base.js, snake.js, web.js
New division added for progress bar with 2 components: myprogress and mybar (the progress bar works but doesn’t go up in increments like it should and instead just fills from 0 to 100)
Close off anything that still needs closed off

body, html {
 width: 100%;
 height: 100%;
 background:      #121212;
 text-align:      center;
 color:           #fff;
 display:         flex;
 justify-content: center;
 align-items:     center;
 overflow: hidden;
 margin: 0;
 padding: 0;
 }

#startScreen
{
    margin: auto;
    margin-top: 50px;
    position: relative;
    width: 300px;
    height: 200px;
    background-color: whitesmoke;
    color: #333
    text-align: center;
    border: 8px solid;
    border-radius: 10px;
    box-shadow: 0 1px 32px white;
    transform: rotate(0) scale(0);
    animation: boom 1s linear;
    animation-fill-mode: forwards;
}

@keyframes boom
{
    100%
    {
        transform: rotate(1080deg)scale(1);
    }
}

#startScreen div
{
    height: 50%;
}

#startScreen h1 {
  position:absolute;
  width: 100%;
  color: #222;
  font-size: 40px;
  text-shadow: 0 1px 2px;
  animation: pulse 6s linear infinite;
}

@keyframes pulse {
  50% {
    transform: scale(1.1);
  }
}

#startScreen p {
  font-weight: bold;
}

#startScreen span {
  position: relative;
  font-size: 18px;
  animation: up 2s linear infinite;
  animation-delay: 3s;
}

@keyframes up {
  25% {
    top: -4px;
  }
}

#gameOn {
  font-size: 100px;
  text-align: center;
  text-shadow: 0 2px 0 limegreen, 0 1px 64px white;
  animation: pulse 1.5s linear infinite;
}

#myBar
{
  width: 10%;
  height: 30px;
  background-color: #4CAF50;
  text-align: center; /* To center it horizontally (if you want) */
  line-height: 30px; /* To center it vertically */
  color: white;
}

 canvas
 {
 position: absolute;
 width: 100%;
 height: 100%;
 background: #eee;
 display: block;
 margin: 0 auto;
 }

 * {
 margin:0;
 padding:0;
 }

gameCSS.css (will only go over important sections)
Set width and height to 100%
Set background colour
Align text centre
Make display flexible
Set content to be in the centred
Margin and padding both set to 0

Section for StartScreen
Sets up variables for how start screen is to be shown
Section for myBar
Set up variables for dynamic progress bar
Set up canvas


const adjust    = n => f => xs => mapi(x => i => i == n ? f(x) : x)(xs)
const dropFirst = xs => xs.slice(1)
const dropLast  = xs => xs.slice(0, xs.length - 1)
const id        = x => x
const k         = x => y => x
const map       = f => xs => xs.map(f)
const mapi      = f => xs => xs.map((x, i) => f(x)(i))
const merge     = o1 => o2 => Object.assign({}, o1, o2)
const mod       = x => y => ((y % x) + x) % x // http://bit.ly/2oF4mQ7
const objOf     = k => v => { var o = {}; o[k] = v; return o }
const pipe      = (...fns) => x => [...fns].reduce((acc, f) => f(acc), x)
const prop      = k => o => o[k]
const range     = n => m => Array.apply(null, Array(m - n)).map((_, i) => n + i)
const rep       = c => n => map(k(c))(range(0)(n))
const rnd       = min => max => Math.floor(Math.random() * max) + min
const spec      = o => x => Object.keys(o)
  .map(k => objOf(k)(o[k](x)))
  .reduce((acc, o) => Object.assign(acc, o))

module.exports = { adjust, dropFirst, dropLast, id, k, map, merge, mod, objOf, pipe, prop, range, rep, rnd, spec }




base.js
This section is just a bunch of commands that other scripts will use and saves me from needing a library.

const readline = require('readline');
const Snake    = require('./snake');
const base     = require('./base');
Object.getOwnPropertyNames(base).map(p => global[p] = base[p]);

// Mutable state
let State = Snake.initialState();

// Matrix operations
const Matrix = {
  make:      table => rep(rep('.')(table.cols))(table.rows),
  set:       val   => pos => adjust(pos.y)(adjust(pos.x)(k(val))),
  addSnake:  state => pipe(...map(Matrix.set('X'))(state.snake)),
  addApple:  state => Matrix.set('o')(state.apple),
  addCrash:  state => state.snake.length == 0 ? map(map(k('#'))) : id,
  toString:  xsxs  => xsxs.map(xs => xs.join(' ')).join('\r\n'),
  fromState: state => pipe(
    Matrix.make,
    Matrix.addSnake(state),
    Matrix.addApple(state),
    Matrix.addCrash(state),
  )(state)
}

// Key events
readline.emitKeypressEvents(process.stdin);
process.stdin.setRawMode(true);
process.stdin.on('keypress', (str, key) => {
  if (key.ctrl && key.name === 'c') process.exit()
  switch (key.name.toUpperCase()) {
    case 'W': case 'K': case 'UP':    State = Snake.enqueue(State, Snake.NORTH); break
    case 'A': case 'H': case 'LEFT':  State = Snake.enqueue(State, Snake.WEST);  break
    case 'S': case 'J': case 'DOWN':  State = Snake.enqueue(State, Snake.SOUTH); break
    case 'D': case 'L': case 'RIGHT': State = Snake.enqueue(State, Snake.EAST);  break
  }
});

// Game loop
const show = () => console.log('\x1Bc' + Matrix.toString(Matrix.fromState(State)))
const step = () => State = Snake.next(State)

// Main
setInterval(() => { step(); show() }, 80)

cli.js
this script holds a few key operations these being:
matrix operations that cover various operations coving areas such as the snake, apples and collision with the snake
key events that cover the movement of the snake through keypresses and updates the state of the snake to north, west, south or east respectively.

const base = require('./base')
Object.getOwnPropertyNames(base).map(p => global[p] = base[p])

// Constants
const NORTH = { x: 0, y:-1 }
const SOUTH = { x: 0, y: 1 }
const EAST  = { x: 1, y: 0 }
const WEST  = { x:-1, y: 0 }

// Point operations
const pointEq = p1 => p2 => p1.x == p2.x && p1.y == p2.y

// Booleans
const willEat   = state => pointEq(nextHead(state))(state.apple)
const willCrash = state => state.snake.find(pointEq(nextHead(state)))
const validMove = move => state =>
  state.moves[0].x + move.x != 0 || state.moves[0].y + move.y != 0

// Next values based on state
const nextMoves = state => state.moves.length > 1 ? dropFirst(state.moves) : state.moves
const nextApple = state => willEat(state) ? rndPos(state) : state.apple
const nextHead  = state => state.snake.length == 0
  ? { x: 2, y: 2 }
  : {
    x: mod(state.cols)(state.snake[0].x + state.moves[0].x),
    y: mod(state.rows)(state.snake[0].y + state.moves[0].y)
  }
const nextSnake = state => willCrash(state)
  ? []
  : (willEat(state)
    ? [nextHead(state)].concat(state.snake)
    : [nextHead(state)].concat(dropLast(state.snake)))

// Randomness
const rndPos = table => ({
  x: rnd(0)(table.cols - 1),
  y: rnd(0)(table.rows - 1)
})

// Initial state
const initialState = () => ({
  cols:  20,
  rows:  14,
  moves: [EAST],
  snake: [],
  apple: { x: 16, y: 2 },
})

const next = spec({
  rows:  prop('rows'),
  cols:  prop('cols'),
  moves: nextMoves,
  snake: nextSnake,
  apple: nextApple
})

const enqueue = (state, move) => validMove(move)(state)
  ? merge(state)({ moves: state.moves.concat([move]) })
  : state

module.exports = { EAST, NORTH, SOUTH, WEST, initialState, enqueue, next, }

snake.js

north, south, east and west are all defined here in order to properly change the direction of the snake
set booleans that will affect what will/can happen in the next frame
set some values based on the next state, for example if the snake will eat the apple generate a new random position and update the state of the apple
generate a random number for x and y dependant on how many columns and rows there are
declare the initial state of the game, so how many columns and rows, the initial position of the snake and apple and the direction the snake is moving

    const canvas = document.getElementById('canvas')
    const ctx = canvas.getContext('2d')



    // this creates the start screen
    function myFunction()
    {


            // used to record last length of the snake
            var lastLength = 0;

            // set score up
            var score = 0;

            //used to pause the game?
            var died = 0;

            //load image
            const foodImg = new Image();
            foodImg.src = "food.png";

            const snakeImg = new Image();
            snakeImg.src = "pepe.png";

            const sovietImg = new Image();
            sovietImg.src = "soviet.png";

            const gameOver = new Image();
            gameOver.src = "gameover.jpg";

            //load audio and assign files to names
            let dead = new Audio();
            let eat = new Audio();
            let annoy = new Audio();
            let gg = new Audio();

            dead.src = "audio/dead.mp3";
            eat.src = "audio/eat.mp3";
            annoy.src = "audio/annoy.mp3";
            gg.src = "audio/moti.mp3";



            // this is what activates the progress bar it was originally meant to be used for score
            function move()
            {
              var elem = document.getElementById("myBar");
              var width = 1;
              var id = setInterval(frame, 1);
              function frame()
              {
                if (width >= 100)
                {
                  clearInterval(id);
                }
                 else
                {
                  width++;
                  elem.style.width = width + '%';
                  elem.innerHTML = width * 1 + '%';
                }
              }
            }

            //  this should draw the score to the screen
            function drawScore()
            {
                ctx.font = "16px Arial";
                ctx.fillStyle = "#0095DD";
                ctx.fillText("Score: " + score, 8, 20);
            }

            function splashIO (event)
            {  // function to start the game when IO is correct
                // check for the correct events
                if(event.type === "click" || (event.type === "keydown" && event.code === "Enter")){
                     // remove events
                     canvas.removeEventListener("click",splashIO);
                     canvas.removeEventListener("keydown",splashIO);
                     gameStates.current = gameStates.startGame;

                     score = new component("30px", "Console", "black", 280, 40, "text");
                }
            }

            // Mutable state
            let state = initialState()

            // Position helpers
            const x = c => Math.round(c * canvas.width / state.cols)
            const y = r => Math.round(r * canvas.height / state.rows)

            // Game loop draw
            const draw = () =>
            {

                if(state.snake.length > 0)
                {
                  // fill background with image
                  var pat=ctx.createPattern(sovietImg,"repeat");
                  ctx.rect(0, 0, canvas.width, canvas.height);
                  ctx.fillStyle=pat;
                  ctx.fill();

                  // draw snake
                  ctx.fillStyle = 'rgb(0,200,50)'
                  state.snake.map(p => ctx.fillRect(x(p.x), y(p.y), x(1), y(1)));
                  state.snake.map(p => ctx.drawImage(snakeImg, x(p.x), y(p.y), x(1), y(1)));

                  // draw apple
                  ctx.fillStyle = 'rgb(255,255,0)'
                  ctx.fillRect(x(state.apple.x), y(state.apple.y), x(1), y(1));
                  ctx.drawImage(foodImg, x(state.apple.x), y(state.apple.y), x(1), y(1));

                  // actually draw the score
                  drawScore();

                }
              // add crash, checks length of snake to see if dead or an action needs to happen
              if (state.snake.length == 0)
              {
              //when the snake dies this is meant to wait for a key press to allow the game to start again
                  if (died = 1)
                  {
                    //somehow in the code this section is always accessed at the start of the game as noted by the audio
                    //this just draws the game over screen
                      var pat=ctx.createPattern(gameOver,"repeat");
                      ctx.rect(0, 0, canvas.width, canvas.height);
                      ctx.fillStyle=pat;
                      ctx.fill();
                      //make sure score is visable
                      drawScore();
                      //play mp3
                      gg.play();
                      //reset death
                      died--;
                  }
              }

              //if snake is bigger aka it ate then score increment, sound play, update lastlength
              if (state.snake.length > lastLength)
              {
                score++;
                move();
                eat.play();
                lastLength++;
              }
              //if no change then play sounds
              if(state.snake.length = lastLength)
              {
                // annoying sound
                annoy.play();
              }
              //if died the play sounds, set died to true
              if(state.snake.length < lastLength)
              {
                died++;
                //gg.play();
              }
            }

            // Game loop update
            const step = t1 => t2 =>
            {
              if (t2 - t1 > 100)
              {
                state = next(state)
                draw()

                window.requestAnimationFrame(step(t2))
              }
              else
              {
                window.requestAnimationFrame(step(t1))
              }
            }



            // Key events
            window.addEventListener('keydown', e =>
            {
              switch (e.key)
              {
                case 'w': case 'h': case 'ArrowUp':    state = enqueue(state, NORTH); break
                case 'a': case 'j': case 'ArrowLeft':  state = enqueue(state, WEST);  break
                case 's': case 'k': case 'ArrowDown':  state = enqueue(state, SOUTH); break
                case 'd': case 'l': case 'ArrowRight': state = enqueue(state, EAST);  break
            }
            })

            // Main
            draw(); window.requestAnimationFrame(step(0))


    }

    /* couldn't get touch controls working
    //touch event listener
        canvas.addEventListener("touchmove", touchXY, true);

        function touchXY(evt)
        {
        evt.preventDefault();
        if(lastPt!=null) {
        var touchX = evt.touches[0].pageX - canvas.offsetLeft;
        var touchY = evt.touches[0].pageY - canvas.offsetTop;
        }
        lastPt = {x:evt.touches[0].pageX, y:evt.touches[0].pageY};
        }

        var lastPt=null;
    */
    /*
          //touch movement maths
              if (snake.position.x + lastPt.x > snake.position.y + lastPt.y)
              {
              if (snake.position.x > lastPt.x)
              {
                  state = enqueue(state, East);
              }
              else
              {
                  state = enqueue(state, WEST);
              }
              }
              if (snake.position.x + lastPt.x < snake.position.y + lastPt.y)
                  {
                  if (snake.position.y > lastPt.y)
                  {
                      state = enqueue(state, SOUTH);
                  }
                  else
                  {
                      state = enqueue(state, NORTH);
                  }
                  }
        }
*/



web.js

This script would be considered the main game script (where the game actually takes place)

The first thing that happens in this script is the canvas is set up for use later

The next big thing that happens the scrip waits for the button that was made in the html to be pressed, this starts the game

Values are initialised, images are loaded and assigned names; audio files are also loaded and assigned names

The next function is the move function, this function is what moves the dynamic progress bar, this is just to show it could be in the game, initially it was going to be used to measure the score, I was unable to get it working correctly as such it simply updates to 100% whenever a apple is eaten.

The drawScore function simply draws the score to the top left of the canvas

The next notable section of the program is the game draw loop in this loop there are several things that are checked:
	Firstly we check if the length of the snake is greater than zero. If it is we draw the background and have it repeat, we draw the snake using the correct image and likewise with the apple and we call to drawScore.
	Next we check if the length of the snake is 0 and then if the snake has died, there is a bug that makes this true for the very first frame of the game, it draws the gameover screen, the score and plays the game over audio then sets died to 0 (the bug occurs of the first frame as when the games loads the snake has length 0 for one frame.
	Next we do a few checks with the length of the snake versus the last recorded length of the snake, if the snake is bigger than before the score is incremented, function move is called, audio is played and last length is incremented. If the snake has stayed the same length as before then a sound is played. If the snake is smaller, which happens when the snake dies died is incremented to 1. 

Next we have the Game loop update, in here we update game state and call the draw function whilst requesting the next animation frame

Then we have a repeat of the event listener function for keydown which updates the state relevant to the direction of the snake

Now the last part of this script is code that has been commented out, these would have been the touch controls where the xy position of the touch would be taken and then compared to the xy position of the snake and the direction of the snake would be decided from whichever value was highest, say you wanted the snake to turn east then you would have to tap the screen to the right of the snake but not higher than the snake. I felt that this should be left in to show my attempt at integrating touch controls
