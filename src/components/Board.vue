<template>
  <main class="playground">
    <h3 class="playground--message" v-if="!winner">Ходить {{ player }}</h3>
    <div class="board">
      <div v-for="(row, x) in board" :key="x" class="row">
        <div
          v-for="(cell, y) in row"
          :key="y"
          @click="makeMove(x, y)"
          class="cell"
        >
          <div :class="cell === 'X' ? 'mark-X' : cell === 'O' ? 'mark-O' : ''">
            {{ cell === "X" ? "X" : cell === "O" ? "O" : "" }}
          </div>
        </div>
      </div>
    </div>
    <div class="playground--result" v-if="winner">
      <h2 v-if="winner === 'draw'">Draw!</h2>
      <h2 v-else>Player '{{ winner }}' won!</h2>
    </div>
    <button class="reset-button" @click="resetBoard">Reset game</button>
  </main>
</template>

<script setup>
import { computed, ref } from "vue";

const player = ref("X");
const board = ref([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""],
]);
const calculateWinner = (squares) => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  let isDraw = true;
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (!squares[a] || !squares[b] || !squares[c]) isDraw=false;
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return isDraw ? "draw" : null;
};

const winner = computed(() => calculateWinner(board.value.flat()));

const makeMove = (x, y) => {
  if (winner.value) return;

  if (board.value[x][y] !== "") return;

  board.value[x][y] = player.value;

  player.value = player.value === "X" ? "O" : "X";
};

const resetBoard = () => {
  board.value = [
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
  ];
  player.value = "X";
};
</script>

<style>
.board {
  border: 1px solid rgb(175, 5, 175);
  width:600px;
  height: 600px;
  display: flex;
  flex-direction: column;
  background: rgba(34,34,34, 0.289);
  backdrop-filter: blur(30px);
}

.row {
  border: 1px solid rgb(175, 5, 175);
  height: 200px;
  display: flex;
}

.cell {
  border: 1px solid rgb(175, 5, 175);
  height: 200px;
  width: 200px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 72px;
  font-weight: 700;
}

.mark-X {
  color: rgb(196, 39, 196);
}

.mark-O {
  color:rgb(255, 196, 0);
}

.reset-button {
  height: 50px;
  width: 200px;
  margin-top: 20px;
  background: transparent;
  border: 3px solid purple;
  border-radius: 10px;
  font-size: 24px;
  font-style: italic;
}
</style>