# Part1
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flash Card Matching Game</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
        }

        .game-board {
            display: grid;
            grid-template-columns: repeat(4, 100px);
            gap: 10px;
            justify-content: center;
            margin-top: 20px;
        }

        .card {
            width: 100px;
            height: 140px;
            background-color: #4CAF50;
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 18px;
            cursor: pointer;
            border-radius: 5px;
            user-select: none;
        }

        .card.flipped {
            background-color: #f1f1f1;
            color: black;
        }

        .card-text {
            font-size: 20px;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <h1>Flash Card Matching Game</h1>
    <p>Click on two cards to find a match!</p>
    <div class="game-board" id="gameBoard"></div>

    <script>
        const cards = [
            { term: "HTML", definition: "Markup Language" },
            { term: "CSS", definition: "Styles Websites" },
            { term: "JS", definition: "Adds Interactivity" },
            { term: "Python", definition: "Programming Language" }
        ];

        let gameCards = [...cards, ...cards].sort(() => Math.random() - 0.5);
        let firstCard = null;
        let secondCard = null;
        let lockBoard = false;

        const gameBoard = document.getElementById("gameBoard");
        gameCards.forEach((item, index) => {
            const card = document.createElement("div");
            card.classList.add("card");
            card.dataset.value = item.term; 
            card.textContent = "?";
            card.addEventListener("click", flipCard);
            gameBoard.appendChild(card);
        });

        function flipCard() {
            if (lockBoard || this === firstCard) return;

            this.classList.add("flipped");
            this.textContent = this.dataset.value;

            if (!firstCard) {
                firstCard = this;
                return;
            }

            secondCard = this;
            lockBoard = true;
            checkMatch();
        }

        function checkMatch() {
            let isMatch = firstCard.dataset.value === secondCard.dataset.value;

            if (isMatch) {
                firstCard.removeEventListener("click", flipCard);
                secondCard.removeEventListener("click", flipCard);
                resetBoard();
            } else {
                setTimeout(() => {
                    firstCard.classList.remove("flipped");
                    secondCard.classList.remove("flipped");
                    firstCard.textContent = "?";
                    secondCard.textContent = "?";
                    resetBoard();
                }, 1000);
            }
        }

        function resetBoard() {
            [firstCard, secondCard, lockBoard] = [null, null, false];
        }
    </script>
</body>
</html>
