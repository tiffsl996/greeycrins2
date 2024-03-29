---
comments: true
layout: post
title: Sprite Demo
description: getting familiar with sprites
type: hacks
courses: { compsci: {week: 3} }
---

<body>
    <div>
        <canvas id="spriteContainer"> <!-- Within the base div is a canvas. An HTML canvas is used only for graphics. It allows the user to access some basic functions related to the image created on the canvas (including animation) -->
            <img id="hamsterSprite" src="{{site.baseurl}}/images/PC Computer - Box Critters - Hamster.png">  // change sprite here
        </canvas>
        <div id="controls"> <!--basic radio buttons which can be used to check whether each individual animaiton works -->
            <input type="radio" name="animation" id="jumping" checked>
            <label for="jumping">Jumping</label><br>
            <input type="radio" name="animation" id="side">
            <label for="side">Side</label><br>
            <input type="radio" name="animation" id="back">
            <label for="back">Back</label><br>
        </div>
    </div>
</body>

<script>
    // start on page load
    window.addEventListener('load', function () {
        const canvas = document.getElementById('spriteContainer');
        const ctx = canvas.getContext('2d');
        const SPRITE_WIDTH = 90;  // matches sprite pixel width
        const SPRITE_HEIGHT = 120; // matches sprite pixel height
        const FRAME_LIMIT = 4;  // matches number of frames per sprite row, this code assume each row is same

        const SCALE_FACTOR = 2;  // control size of sprite on canvas
        canvas.width = SPRITE_WIDTH * SCALE_FACTOR;
        canvas.height = SPRITE_HEIGHT * SCALE_FACTOR;

class Hamster {
    constructor() {
        this.image = document.getElementById("hamsterSprite");
        this.x = 30; // Set an initial value for x (adjust as needed)
        this.y = 50; // Set an initial value for y (adjust as needed)
        this.minFrame = 0;
        this.maxFrame = FRAME_LIMIT;
        this.frameX = 0;
        this.frameY = 0;
    }
    
            // draw dog object
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
                    this.frameX = 0;
                }
            }
        }

        // dog object
        const hamster = new Hamster();

        // update frameY of dog object, action from idle, bark, walk radio control
        const controls = document.getElementById('controls');
        controls.addEventListener('click', function (event) {
            if (event.target.tagName === 'INPUT') {
                const selectedAnimation = event.target.id;
                switch (selectedAnimation) {
                    case 'jumping':
                        hamster.frameY = 0;
                        break;
                    case 'side':
                        hamster.frameY = 1;
                        break;
                    case 'back':
                        hamster.frameY = 2;
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
            hamster.draw(ctx);

            // Updates the `frameX` property to prepare for the next frame in the sprite sheet.
            hamster.update();

            // Uses `requestAnimationFrame` to synchronize the animation loop with the display's refresh rate,
            // ensuring smooth visuals.
            setTimeout(function () {
        // Use `requestAnimationFrame` to continue the animation loop
        requestAnimationFrame(animate);
    }, 500 / 10); // Adjust the divisor to set the desired frames per second
}
// run 1st animate
animate();
    });
</script>