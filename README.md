# First-repository-
<div id="hud">
    <div class="health-bar" id="player1-health"></div>
    <div class="health-bar" id="player2-health"></div>
</div>

<canvas id="gameCanvas"></canvas>

<script>
    // Setup canvas
    const canvas = document.getElementById("gameCanvas");
    const ctx = canvas.getContext("2d");

    canvas.width = 800;
    canvas.height = 400;

    // Load images
    const fighter1Img = new Image();
    fighter1Img.src = "https://i.imgur.com/qINwEdO.png"; // Replace with your sprite

    const fighter2Img = new Image();
    fighter2Img.src = "https://i.imgur.com/XSTVMSq.png"; // Replace with your sprite

    // Fighter class
    class Fighter {
        constructor(x, y, img) {
            this.x = x;
            this.y = y;
            this.width = 50;
            this.height = 100;
            this.health = 100;
            this.speed = 5;
            this.velocityY = 0;
            this.gravity = 0.5;
            this.img = img;
            this.isAttacking = false;
        }

        draw() {
            ctx.drawImage(this.img, this.x, this.y, this.width, this.height);
        }

        update() {
            this.y += this.velocityY;
            if (this.y + this.height < canvas.height) {
                this.velocityY += this.gravity;
            } else {
                this.y = canvas.height - this.height;
                this.velocityY = 0;
            }
            this.draw();
        }

        attack(opponent) {
            if (this.isAttacking) return;
            this.isAttacking = true;
            setTimeout(() => this.isAttacking = false, 500);

            // Check collision
            if (this.x + this.width >= opponent.x && this.x <= opponent.x + opponent.width) {
                opponent.health -= 10;
                updateHealthBars();
            }
        }
    }

    // Initialize fighters
    const player1 = new Fighter(100, canvas.height - 100, fighter1Img);
    const player2 = new Fighter(600, canvas.height - 100, fighter2Img);

    // Update health bars
    function updateHealthBars() {
        document.getElementById("player1-health").style.width = player1.health + "%";
        document.getElementById("player2-health").style.width = player2.health + "%";

        if (player1.health <= 0 || player2.health <= 0) {
            alert(player1.health <= 0 ? "Player 2 Wins!" : "Player 1 Wins!");
            location.reload();
        }
    }

    // Game loop
    function animate() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        player1.update();
        player2.update();
        requestAnimationFrame(animate);
    }

    animate();

    // Controls
    const keys = {};
    window.addEventListener("keydown", (e) => {
        keys[e.key] = true;
        if (e.key === "w" && player1.y + player1.height >= canvas.height