---
layout: portfolio
title: Sets
---

<div class='main'>
    <div class='card' id='card1' onclick='clicked(1)'></div>
    <div class='card' id='card2' onclick='clicked(2)'></div>
    <div class='card' id='card3' onclick='clicked(3)'></div>
    <div class='card' id='card4' onclick='clicked(4)'></div>
    <div class='card' id='card5' onclick='clicked(5)'></div>
    <div class='card' id='card6' onclick='clicked(6)'></div>
    <div class='card' id='card7' onclick='clicked(7)'></div>
    <div class='card' id='card8' onclick='clicked(8)'></div>
    <div class='card' id='card9' onclick='clicked(9)'></div>
    <p><span></span> possible sets</p>
    
</div>

### Instructions

Set is a card game which consists of 27 cards varying in three features: shape (circle, triangle, square); fill (solid, striped, or open); and color (red, green, or blue).

A set consists of three cards satisfying all of these conditions:

- They all have the same shape or have three different shapes.
- They all have the same fill or have three different fills.
- They all have the same color or have three different colors.

-- Adapted from [Wikipedia](https://en.wikipedia.org/wiki/Set_(game))

#### Example
![Example]({{ site.baseurl }}/portfolio/images/sets/example.png)

This is a set because

- All cards have the same shape (triangle)
- All cards have 3 different fills
- All cards have 3 different colours

#### Non-Example
![Non-Example]({{ site.baseurl }}/portfolio/images/sets/non-example.png)

This is not a set because 2 of the cards have triangles but the last has a square.

### Keyboard Shortcuts
![Keyboard Shortcuts]({{ site.baseurl }}/portfolio/images/sets/keyboard-shortcuts.png)

### Inspiration
Graphics were inspired by [this](https://play.google.com/store/apps/details?id=nl.borkoek.set) version of the game. 

<style>
    .main {
        width: 510px;
        margin: 0 auto;
    }
    .card {
        float: left;
        height: 150px;
        width: 150px;
        border-radius: 7px;
        border: 2px solid white;
        box-shadow: 2px 2px 2px #888888;
        margin: 10px;

        -webkit-box-sizing: border-box;
        -moz-box-sizing: border-box;
        box-sizing: border-box;

        background: #FFF left top;
        background-size: 148px 148px;
    }

    .card:hover, .active {
        border: 2px solid red;
        cursor: pointer;
    }

</style>
<script>
 /*global $*/

 var board = [ [],
    [], [], [],
    [], [], [],
    [], [], []
 ];

 // attach the .equals method to Array's prototype to call it on any array
 Array.prototype.equals = function (array) {
    // if the other array is a falsy value, return
    if (!array)
        return false;

    // compare lengths - can save a lot of time 
    if (this.length != array.length)
        return false;

    for (var i = 0, l=this.length; i < l; i++) {
        // Check if we have nested arrays
        if (this[i] instanceof Array && array[i] instanceof Array) {
            // recurse into the nested arrays
            if (!this[i].equals(array[i]))
                return false;       
        }           
        else if (this[i] != array[i]) { 
            // Warning - two different object instances will never be equal: {x:20} != {x:20}
            return false;   
        }           
    }       
    return true;
 };

 Array.prototype.allSame = function() {
    for(var i = 1; i < this.length; i++) {
       if(this[i] !== this[0])
       return false;
    }
    return true;
 }

 Array.prototype.allDiff = function() {
    this.sort();
    for(var i = 1; i < this.length; i++) {
       if(this[i] == this[i-1])
       return false;
    }
    return true;
 }

 // Hide method from for-in loops
 Object.defineProperty(Array.prototype, "equals", {enumerable: false});

 function newCard(id) {
    var colour, shape, fill, card;
    function drawCard() {
       colour = Math.floor(Math.random() * 3);
       shape = Math.floor(Math.random() * 3);
       fill = Math.floor(Math.random() * 3);

       card = [colour, shape, fill];
    }
    drawCard();

    var check = true;
    var duplicates;

    while (check) {
       duplicates = 0;
       for (var j = 1; j <= 9; j++) {
          while (board[j].equals(card)) {
             console.log('duplicate card: '+j+' and '+id);
             drawCard();
             duplicates++;
          }
       }
       if (duplicates == 0) {
          check = false;
       }
    }

    board[id] = card;
    $('#card'+id).css('background-image', 'url(https://xlzior.github.io/portfolio/images/sets/'+card[0]+card[1]+card[2]+'.png)');
 }

 function checkSet(a, b, c) {
    var colours, shapes, fills;
    var array = [colours, shapes, fills];

    var isSet = true;

    for (var i = 0; i < 3; i++) {
       array[i] = [a[i], b[i], c[i]];
       if (!(array[i].allDiff() || array[i].allSame())) {
          isSet = false;
       }
    }
    return isSet;
 }

 function countSets() {
    var pSets = 0;
    var loops = 0;
    for (var m = 1; m <= 9; m++) {
       for (var n = m+1; n <= 9; n++) {
          for (var o = n+1; o <= 9; o++) {
             if (checkSet(board[m], board[n], board[o])) {
                pSets++;
             }
          }
       }
    }
    $('span').text(pSets);
 }

 function clicked(id) {
    $("#card"+id).toggleClass('active');

    if ($('.active').length == 3) {

       // Get the active card numbers
       var a = $('.active')[0].id.substr(4);
       var b = $('.active')[1].id.substr(4);
       var c = $('.active')[2].id.substr(4);

       // checkSet for active cards
       if (checkSet(board[a], board[b], board[c])) {
          console.log(a+" "+b+" "+c);
          // reset card values and generate a new card
          board[a] = [];
          board[b] = [];
          board[c] = [];
          newCard(a);
          newCard(b);
          newCard(c);
       }

       countSets();

       setTimeout(function() {
          $('.active').removeClass('active');
       }, 100);
    }
 }

 $(document).ready(function() {
    // Generate first board layout
    for (var i = 1; i <= 9; i++) {
       newCard(i);
    }

    countSets();

    $(document).keydown(function(key) {
       key = String.fromCharCode(key.which).toLowerCase();
       var keys = ['0', 'y', 'u', 'i', 'h', 'j', 'k', 'b', 'n', 'm'];
       var index = keys.indexOf(key);

       clicked(index);
    });
 });
</script>