<template>
  <div>
    <main class="pt-16 text-center">
      <h1 class="mb-8 text-3xl font-bold uppercase text-white">Tic Tac Toe</h1>
      <h3 class="text-xl mb-4 text-slate-200">
        ~ Player {{ player }}'s turn ~
      </h3>
      <div class="flex flex-col items-center mb-8 pt-16">
        <div v-for="(row, x) in board" :key="x" class="flex">
          <div
            v-for="(cell, y) in row"
            :key="y"
            @click="MakeMove(x, y)"
            :class="`border border-white w-24 h-24 hover:bg-gray-700 hover:scale-105 hover:rounded-lg flex items-center justify-center material-icons-outlined text-4xl cursor-pointer ${
              cell === 'X' ? 'text-pink-500' : 'text-blue-400'
            }`"
          >
            {{ cell === "X" ? "X" : cell === "O" ? "O" : "" }}
          </div>
        </div>
      </div>
      <div class="text-center pt-8">
        <h2
          v-if="winner"
          class="text-6xl font-bold mb-8"
          :class="winner == 'X' ? 'text-pink-500' : 'text-blue-400'"
        >
          Player '{{ winner }}' wins!
        </h2>
        <div class="pt-8">
          <button
            type="button"
            @click="ResetGame"
            class="
              text-white
              bg-[#24292F]
              hover:bg-[#24292F]/60 hover:scale-95
              focus:ring-4 focus:outline-none focus:ring-[#24292F]/50
              font-medium
              rounded-lg
              text-sm
              px-10
              py-2.5
              text-center
              inline-flex
              items-center
              mr-2
              mb-2
            "
          >
            <svg
              class="mr-2 -ml-1 w-4 h-4"
              aria-hidden="true"
              focusable="false"
              fill="none"
              data-prefix="fab"
              data-icon="refresh"
              role="img"
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 24 24"
              stroke="currentColor"
              stroke-width="2"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15"
              ></path>
            </svg>
            Reset
          </button>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
const player = ref("X");
const board = ref([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""],
]);
const CalculateWinner = (board) => {
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
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (board[a] && board[a] === board[b] && board[a] === board[c]) {
      return board[a];
    }
  }
  return null;
};
const winner = computed(() => CalculateWinner(board.value.flat()));
const closeGame = false;
const MakeMove = (x, y) => {
  if (winner.value) return;
  if (board.value[x][y]) return;
  board.value[x][y] = player.value;
  player.value = player.value === "X" ? "O" : "X";
};
const ResetGame = () => {
  board.value = [
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
  ];
  player.value = "X";
};
</script>