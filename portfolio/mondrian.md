---
layout: portfolio
title: Mondrian
---

<script src="https://xlzior.github.io/portfolio/jquery.ui.touch-punch.min.js"></script>

<div class="main">
    <section id="element-drawer">
        <div class="element-wrapper original">
            <div class="element blue box "></div>
        </div>
        <div class="element-wrapper original">
            <div class="element red box"></div>
        </div>
        <div class="element-wrapper original">
            <div class="element yellow box"></div>
        </div>
        <div class="element-wrapper original">
            <div class="element vertical divider"></div>
        </div>
        <div class="element-wrapper original">
            <div class="element horizontal divider"></div>
        </div>
        <p>drag the boxes onto the canvas</p>
    </section>
    
    <section id="container">
        <div id="canvas"></div>
    </section>
    
</div>

### Instructions
Piet Mondrian was a Dutch painter renowned for his [grid-based paintings](https://www.google.com.sg/search?q=mondrian&tbm=isch) filled with primary colours, black, grey or white. This webpage provides users with a drag-and-drop interface to create their own Mondrian-inspired artwork.

Drag and drop elements from the left panel to the canvas. Elements on the canvas may be resized by dragging the corners of the element.

<style>
    .main {
        background: #333;
        font-size: 24px;
        color: #333;
        letter-spacing: 0.5px;
        line-height: 1.4;
        -webkit-box-sizing: border-box;
        box-sizing: border-box;
        padding: 10px;
        display: flex;
        min-height: 700px;
        margin: 0 auto;
    }

    /* ELEMENT DRAWER */

    #element-drawer {
       position: relative;
       box-sizing: content-box;
       width: 0%;
       background: #fdfdfd;
       display: flex;
       flex-wrap: wrap;
       padding: 10px;
       left: 0;
       width: 100px;
       margin-right: 10px;
    }

    #element-drawer p {
       margin: 0px 10px;
       position: absolute;
       left: 110%;
       min-width: 400px;
       z-index: 10;
    }

    #element-drawer > div {
       height: 100px;
    }

    .element {
       z-index: 40;
    }

    .box {
       margin: 0 auto;
       height: 100px;
       width: 100px;
    }

    .box:hover {
       cursor: pointer;
    }

    .divider {
       background: black;
    }

    .divider:hover {
       cursor: pointer;
    }

    .blue { background: #1D53E9; }
    .red { background: #E81B08; }
    .yellow { background: #F5D513; }
    .black { background: black; }
    .white { background: white; }
    .horizontal { height: 20px; width: 100px; }
    .vertical { height: 100px; width: 20px; }


    /* CONTAINER */

    #container {
       width: 100%;
       background: #ddd;
       position: relative;
    }

    #canvas {
        height: 500.5px;
        width: 500.99999px;
        background: #fafafa;
        margin: 0 auto;
        box-shadow: 7px 7px 0px #8d8d8d;
        box-sizing: border-box;
        position: relative;
        top: 50%;
        transform: translateY(-50%);
    }

    .element-wrapper {
       display: inline-block;
       z-index: 20;
    }

    .row {
       display: block;
       height: 20px;
       z-index: 0;
    }

    .grid {
       position: relative;
       top: -4px;
       border: 1px solid #eee;
       display: inline-block;
       height: 20px;
       width: 20px;
       box-sizing: border-box;
       z-index: 0;
    }

    .ui-resizable { position: relative;}
    .ui-resizable-handle { position: absolute;font-size: 0.1px; display: block; }
    .ui-resizable-disabled .ui-resizable-handle, .ui-resizable-autohide .ui-resizable-handle { display: none; }
    .ui-resizable-n { cursor: n-resize; height: 7px; width: 100%; top: -5px; left: 0; }
    .ui-resizable-s { cursor: s-resize; height: 7px; width: 100%; bottom: -5px; left: 0; }
    .ui-resizable-e { cursor: e-resize; width: 7px; right: -5px; top: 0; height: 100%; }
    .ui-resizable-w { cursor: w-resize; width: 7px; left: -5px; top: 0; height: 100%; }
    .ui-resizable-se { cursor: se-resize; width: 12px; height: 12px; right: 1px; bottom: 1px; }
    .ui-resizable-sw { cursor: sw-resize; width: 9px; height: 9px; left: -5px; bottom: -5px; }
    .ui-resizable-nw { cursor: nw-resize; width: 9px; height: 9px; left: -5px; top: -5px; }
    .ui-resizable-ne { cursor: ne-resize; width: 9px; height: 9px; right: -5px; top: -5px;}


    /* BUTTONS */

    #button-wrapper {
        position: absolute;
        bottom: 20px;
        left: 10px;
        display: none;
    }

    .button {
        text-decoration: none;
        padding: 12px 22px;
        border: 1px solid #1F3E19;
        left: 10px;
        font-size: 18px;
    }

    .button:hover {
        cursor: pointer;
        background: #1F3E19;
        color: #FFF;
    }
</style>
<script>
    $(document).ready(function() {
       for (var i = 0; i < 25; i++) {
          var string = "<div class='row'>";
          for (var j = 0; j < 25; j++) {
             string += "<div class='grid'></div>"
          }
          string += "</div>"
          $('#canvas').append(string);
       }

       $('.original').draggable({
          cursor: "move",
          scroll: false,
          appendTo: "#canvas",
          helper: "clone",
          snap: ".grid"
       });

       $('#canvas').droppable({
          drop: function(e, ui) {
             if ($(ui.helper).clone().hasClass('original')) {
                $(this).append($(ui.helper).clone());

                // blanket rules
                var elementwrapper = $(this).find('.element-wrapper');
                var element = $(this).find('.element');
                elementwrapper.removeClass('original').addClass('clone');
                elementwrapper.draggable({
                   cursor: "move",
                   scroll: false,
                   snap: ".grid",
                   containment: "#canvas",
                   stack: "#canvas .element-wrapper"
                });
                element.resizable({
                   grid: 20,
                   containment: '#canvas'
                });

                // specific rules
                var horz = $(this).find('.horizontal');
                horz.resizable( "option", "handles", "e" );

                var vert = $(this).find('.vertical');
                vert.resizable( "option", "handles", "s" );
             }
          }
       });
    });
</script>