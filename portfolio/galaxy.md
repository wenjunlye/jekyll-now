---
layout: portfolio
title: Galaxy
---

<div class="main">
    <div id="helper"></div>
    <div id="translucent"></div>
    <div id="helper2"></div>
</div>

### Inspiration
Inspired by [The Thoughts Room](http://thequietplaceproject.com/thethoughtsroom/) by The Quiet Place Project.

<style>
    @keyframes snow {
      0% {background-position: 0px 0px, 0px 0px, 0px 0px;}
      50% {background-position: 500px 500px, 100px 200px, -100px 150px;}
      100% {background-position: 500px 1000px, 200px 400px, -100px 300px;}
    }

    .main {
       margin: 0;
       padding: 0;
       height: 100%;
       width: 100%;
       box-sizing: border-box;
       background-size: 100vw 100vh;
       background-position: center top;
       background-image: url("https://newevolutiondesigns.com/images/freebies/galaxy-wallpaper-36.jpg");
       background-repeat: no-repeat;
       position: absolute;
    }

    p {
       display: inline;
       position: relative;
    }

    #input, #helper, #translucent, #helper2 {
       position: absolute;
       top: 50%;
       left: 50%;
       transform: translateX(-50%) translateY(-50%);
       margin: 0 auto;

       height: 26px;
       width: 60%;
       padding: 15px;

       font-family: 'Roboto Mono', monospace;
       font-size: 20px;
       color: white;
       letter-spacing: 2px;
    }

    #helper {
       height: 40vh;
       top: calc(50% + 18vh);
       z-index: -1;
    }

    #translucent {
       border-bottom: 2px solid #eee;
       background: rgba(0, 0, 0, 0.3);
       z-index: -2;
    }

    #helper2 {
       z-index: -3;
    }
</style>

<script>
    $(document).ready(function(){
       var counter = 1, prevpos = 0;

       $(document).keydown(function(e) {
          // update p to newest input text
          if (e.which <= 90 && e.which >= 65 || e.which == 188 || e.which == 190 || e.which == 222 || e.which == 32) {
             $("#helper").append("<p class='char' id='char"+counter+"'>" + e.key + "</p>");
             $("#helper2").append("<p class='char'>" + e.key + "</p>");

             $('#char'+counter).animate({
                top: "300px",
                opacity : "0",
             }, Math.random() + 5000);

             // if the text overflows the input box
             var nowpos = $("#char"+counter).position().left;
             if (nowpos < prevpos) {
                $('.char').remove();
                prevpos = 0;
             } else {
                prevpos = nowpos;
             }

             counter++;
          } else if (e.which == 13) {
             $('.char').remove();
          }

       });
    });

</script>