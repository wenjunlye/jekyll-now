---
layout: portfolio
title: Lights Out
---

<div class="main">

    <p id="moves">Moves: 0</p>
    <button onclick="gameStart()">Restart</button>
</div>

### Instructions
The objective of this game is to turn off all the lights in the fewest moves possible. A move consists of clicking a light, which toggles the on-off state of the light clicked and the 4 adjacent lights. 

### Inspiration
This project was based on [this](http://mathworld.wolfram.com/LightsOutPuzzle.html) mathematical puzzle.

<style>
     .main {
        display: inline-block;
        width: 78vw;
        max-width: 78vh;
        position: relative;
        left: 50%;
        transform: translateX(-50%);
     }

     .light {
        cursor: pointer;
        background: black;
        border-radius: 50%;
        height: 18vw;
        max-height: 18vh;
        width: 18vw;
        max-width: 18vh;
        float: left;
        margin: 2px;
     }

     .on {
        background: #F5D513;
     }
</style>

<script>
     var rows = 4;
     var bulbs = 5;

     var x, y, moves = 0;

     // Run this function when the game is started/restarted
     function gameStart() {
        console.log("========");
        // Reset game values
        $(".on").removeClass("on");
        moves = 0;
        document.getElementById("moves").innerHTML = "Moves: " + moves;

        // Click (bulbs) random bulbs
        for (var j = 0; j < bulbs; j++) {
           // Choose 1 random bulb and click it
           var x = Math.floor((Math.random() * rows*rows) + 1);
           console.log(x);
           flick(x);
        }
     }

     function coord(index) {
        // Find coordinates from index
        x = (index-1)%rows +1;
        y = parseInt(Math.ceil(index/rows), 10);
     }

     function uncoord(x, y) {
        // Find index from coordinates
        if (x >= 1 && x <= rows && y >= 1 &&  y <= rows) {
           var index = (y-1) * rows + x;
           return index;
        } else {
           return null;
        }
     }

     function flick(index) {
        // Find the coordinates of index
        coord(index);

        // Switch the state of respective bulbs using the coordinates
        $("#" + uncoord(x, y)).toggleClass("on");
        $("#" + uncoord(x-1, y)).toggleClass("on");
        $("#" + uncoord(x+1, y)).toggleClass("on");
        $("#" + uncoord(x, y-1)).toggleClass("on");
        $("#" + uncoord(x, y+1)).toggleClass("on");
     }

     function clicked(index) {
        flick(index);

        // Increment moves by 1
        moves++;
        document.getElementById("moves").innerHTML = "Moves: " + moves;

        if ($(".on").length == 0) {
           alert("You won!");
           gameStart();
        }
     }

     $(document).ready(function() {
        // Create rows*rows lights
        for (var i = 1; i <= rows*rows; i++) {
           $(".main").append('<div class="light" id="' + i + '" onclick="clicked(id)"></div>');
        }

        // Start game
        gameStart();

     });

  </script>