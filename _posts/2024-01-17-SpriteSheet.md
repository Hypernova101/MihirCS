---
toc: false
comments: false
layout: post
title: Sprite Sheet
description: Sprite Sheet Animation
type: tangibles
courses: { compsci: {week: 6} }
permalink: /tangibles/week6
---

<style>
.text-center{
    text-align:center;
    margin-left:auto;
    margin-right:auto;
}
</style>


<body>
    <div class="text-center">
        <canvas id="spriteContainer"> <!-- Within the base div is a canvas. An HTML canvas is used only for graphics. It allows the user to access some basic functions related to the image created on the canvas (including animation) -->
            <img id="cashSprite" src="{{site.baseurl}}/images/CashSprite.png">  // change sprite here
        </canvas>
        <div id="controls"> <!--basic radio buttons which can be used to check whether each individual animaiton works -->
            <input type="radio" name="animation" id="breaking" checked>
            <label for="breaking">Breaking</label><br>
            <input type="radio" name="animation" id="glowing">
            <label for="glowing">Glowing</label><br>
            <input type="radio" name="animation" id="bouncing">
            <label for="bouncing">Bouncing</label><br>
        </div>
    </div>
</body>

<script>
    // start on page load
    window.addEventListener('load', function () {
        const canvas = document.getElementById('spriteContainer');
        const ctx = canvas.getContext('2d');
        const SPRITE_WIDTH = 320;  // matches sprite pixel width
        const SPRITE_HEIGHT = 320; // matches sprite pixel height
        const FRAME_LIMIT = 5;  // matches number of frames per sprite row, this code assume each row is same

        const SCALE_FACTOR = 1;  // control size of sprite on canvas
        canvas.width = SPRITE_WIDTH * SCALE_FACTOR;
        canvas.height = SPRITE_HEIGHT * SCALE_FACTOR;

        class Cash {
            constructor() {
                this.image = document.getElementById("cashSprite");
                this.x = 0;
                this.y = 0;
                this.minFrame = 0;
                this.maxFrame = FRAME_LIMIT;
                this.frameX = 0;
                this.frameY = 0;
            }

            // draw cash object
            draw(context) {
                context.drawImage(
                    this.image,
                    this.frameX * SPRITE_WIDTH,
                    this.frameY * SPRITE_HEIGHT,
                    SPRITE_WIDTH,
                    SPRITE_HEIGHT,
                    this.x,
                    this.y,
                    canvas.width,
                    canvas.height
                );
            }

            // update frameX of object
            update() {
                if (this.frameX < this.maxFrame) {
                    this.frameX++;
                } else {
                    this.frameX = this.minFrame;
                }
            }
        }

        // cash object
        const cash = new Cash();

        // update frameY of cash object, action from breaking, glow, bounce radio control
        const controls = document.getElementById('controls');
        controls.addEventListener('click', function (event) {
            if (event.target.tagName === 'INPUT') {
                const selectedAnimation = event.target.id;
                switch (selectedAnimation) {
                    case 'breaking':
                        cash.frameY = 0;
                        break;
                    case 'glowing':
                        cash.frameY = 1;
                        break;
                    case 'bouncing':
                        cash.frameY = 2;
                        break;
                    default:
                        break;
                }
            }
        });

        // Animation recursive control function
        function animate() {
            // Clears the canvas to remove the previous frame.
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Draws the current frame of the sprite.
            cash.draw(ctx);

            // Updates the `frameX` property to prepare for the next frame in the sprite sheet.
            cash.update();

            // Uses `requestAnimationFrame` to synchronize the animation loop with the display's refresh rate,
            // ensuring smooth visuals.
            setTimeout(function() {

                cash.update();
                
                requestAnimationFrame(animate);
            }, 150);
        }

        // run 1st animate
        animate();
    });
</script>