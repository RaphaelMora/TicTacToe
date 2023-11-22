<template>
  <div class="tic-tac-toe">
    <!-- Conteneur pour le gagnant et le bouton recommencer -->
    <div class="game-info">
      <!-- Affiche gagnant si présent -->
      <p v-if="winner">{{ winnerMessage }}</p>
      <!-- Affiche Match nul si grille pleine sans gagnant -->
      <p v-else-if="isFull">Match nul!</p>
      <!-- Btn Recommencer, affiché si partie finie -->
      <button class="reset" v-if="winner || isFull" @click="resetGame">
        Recommencer
      </button>
    </div>

    <!-- Grille de jeu -->
    <div class="board">
      <!-- Boucle pour chaque cellule de jeu -->
      <div
        v-for="(cell, index) in cells"
        :key="index"
        class="cell"
        @click="makeMove(index)"
      >
        {{ cell }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "TicTacToe",
  data() {
    return {
      cells: Array(9).fill(null),
      currentPlayer: "X",
      winner: null,
      startingPlayer: "X",
      winnerMessage: "",
    };
  },
  computed: {
    isFull() {
      return this.cells.every((cell) => cell !== null);
    },
  },
  methods: {
    makeMove(index) {
      if (!this.cells[index] && !this.winner) {
        this.$set(this.cells, index, this.currentPlayer);

        this.checkForWin(); // Vérifie si le mouvement actuel mène à une victoire

        if (!this.winner) {
          // Change de joueur seulement si aucun gagnant n'a été déterminé
          this.currentPlayer = this.currentPlayer === "X" ? "O" : "X";
        }

        if (!this.winner && this.isFull) {
          // Si aucun gagnant et grille pleine, déclare match nul
          this.winner = "Nobody";
          this.winnerMessage = "Encore magel";
        }
      }
    },
    checkForWin() {
      const lines = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8], // Lignes
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8], // Colonnes
        [0, 4, 8],
        [2, 4, 6], // Diagonales
      ];

      for (let line of lines) {
        const [a, b, c] = line;
        if (
          this.cells[a] &&
          this.cells[a] === this.cells[b] &&
          this.cells[a] === this.cells[c]
        ) {
          this.winner = this.currentPlayer;
          this.winnerMessage =
            this.winner === this.startingPlayer ? "Bravo le veau" : "Cheh !";
          break;
        }
      }
    },
    resetGame() {
      this.cells = Array(9).fill(null);
      this.currentPlayer = "X";
      this.winner = null;
      this.startingPlayer = "X";
      this.winnerMessage = "";
      this.$emit("gameReset");
    },
  },
};
</script>

<style>
.tic-tac-toe {
  display: flex; /* Affichage flexible */
  flex-direction: column; /* Direction colonne */
  align-items: center; /* Centrage éléments */
  background-color: transparent; /* Fond transparent */
  color: royalblue; /* Couleur texte */
  text-shadow: 2px 2px 2px red; /* Ombre de texte rouge */
}
.game-info {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-size: 2rem;
  margin-bottom: 20px; /* Espace entre le conteneur info et le plateau de jeu */
}

.board {
  box-shadow: 5px 10px 10px green; /* Ombre portée verte */
  display: grid; /* Grille pour cellules */
  grid-template-columns: repeat(3, 1fr); /* 3 colonnes égales */
  gap: 5px; /* Espacement cellules */
  margin-bottom: 20px; /* Marge bas */
}

.reset {
  padding: 10px;
}

.cell {
  box-shadow: 5px 10px 10px green; /* Ombre portée verte */
  width: 100px; /* Largeur cellule */
  height: 100px; /* Hauteur cellule */
  display: flex; /* Affichage flexible */
  align-items: center; /* Centrage vertical */
  justify-content: center; /* Centrage horizontal */
  border: 2px solid plum; /* Bordure cellule */
  font-size: 4em; /* Taille symbole */
  cursor: pointer; /* Curseur pointeur */
  text-shadow: 2px 2px 2px red; /* Ombre de texte rouge */
}
</style>
