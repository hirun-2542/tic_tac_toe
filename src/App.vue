<script setup>
import { ref } from "vue";
const board = ref([
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
]);

const gameplay = (row, col) => {
    if (board.value[row][col] === "O") {
        return "cell computer";
    } else if (board.value[row][col] === "X") {
        return "cell player";
    } else {
        return "cell";
    }
};
const handleClick = (row, col) => {
    if (board.value[row][col] === "") {
        board.value[row][col] = "X";
        if (checkWin("X")) {
            setTimeout(() => {
                alert("You win!");
                reset();
            }, 500);
        } else {
            setTimeout(() => {
                computerMove();
            }, 500);
        }
    }
};

const checkWin = (name) => {
    const b = board.value;
    return (
        (b[0][0] === name && b[0][1] === name && b[0][2] === name) ||
        (b[1][0] === name && b[1][1] === name && b[1][2] === name) ||
        (b[2][0] === name && b[2][1] === name && b[2][2] === name) ||
        (b[0][0] === name && b[1][0] === name && b[2][0] === name) ||
        (b[0][1] === name && b[1][1] === name && b[2][1] === name) ||
        (b[0][2] === name && b[1][2] === name && b[2][2] === name) ||
        (b[0][0] === name && b[1][1] === name && b[2][2] === name) ||
        (b[0][2] === name && b[1][1] === name && b[2][0] === name)
    );
};

const computerMove = () => {
    let row = Math.floor(Math.random() * 3);
    let col = Math.floor(Math.random() * 3);
    while (board.value[row][col] !== "") {
        row = Math.floor(Math.random() * 3);
        col = Math.floor(Math.random() * 3);
    }
    board.value[row][col] = "O";
    if (checkWin("O")) {
        setTimeout(() => {
            alert("You lose!");
            reset();
        }, 500);

    }
};

const reset = () => {
    board.value = [
        ["", "", ""],
        ["", "", ""],
        ["", "", ""],
    ];
};
</script>

<template>
    <div class="game">
        <div class="container">
            <div class="board">
                <div class="row" v-for="(row, index) in board" :key="index">
                    <div
                        :class="gameplay(index, num)"
                        @click="handleClick(index, num)"
                        v-for="(col, num) in row"
                        :key="num"
                    >
                        {{ board[index][num] }}
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.game {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #18bcac;
    font-family: cursive;
}

.board {
    width: 500px;
    height: 500px;
    /* border: 5px solid #0da192; */
}
.board .row {
    display: grid;
    width: 100%;
    height: 33.333%;
    grid-template-columns: repeat(3, 1fr);
    border-bottom: 0.5rem solid #0da192;
}

.board .row:last-child {
    border-bottom: none;
}

.board .cell {
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 7rem;
    border-right: 0.5rem solid #0da192;
}

.board .cell:last-child {
    border-right: none;
}

.player {
    color: #535454;
}

.computer {
    color: #eeead2;
}
</style>
