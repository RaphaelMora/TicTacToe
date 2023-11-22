<template>
  <div class="tic-tac-toe">
    <!-- Conteneur pour le gagnant et le bouton recommencer -->
    <div class="game-info">
      <!-- Affiche gagnant si présent -->
      <p v-if="winner">Gagnant: {{ winner }}</p>
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
  name: "TicTacToe", // Nom composant
  data() {
    // Données composant
    return {
      cells: Array(9).fill(null), // 9 cellules initialisées à null
      currentPlayer: "X", // Joueur actuel (X ou O)
      winner: null, // Gagnant (null si aucun)
    };
  },
  computed: {
    isFull() {
      // Vérifie si grille complète
      return this.cells.every((cell) => cell !== null);
    },
  },
  methods: {
    makeMove(index) {
      // Joue un coup
      if (!this.cells[index] && !this.winner) {
        this.$set(this.cells, index, this.currentPlayer); // Met à jour cellule
        if (this.checkForWin()) {
          // Vérifie victoire
          this.winner = this.currentPlayer; // Déclare gagnant
        } else if (this.isFull) {
          // Match nul si grille pleine
          this.winner = "Nobody";
        } else {
          this.currentPlayer = this.currentPlayer === "X" ? "O" : "X"; // Change joueur
        }
      }
    },
    checkForWin() {
      // Vérifie si victoire
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

      return lines.some(([a, b, c]) => {
        return (
          this.cells[a] &&
          this.cells[a] === this.cells[b] &&
          this.cells[a] === this.cells[c]
        );
      });
    },
    resetGame() {
      // Réinitialise jeu
      this.cells = Array(9).fill(null); // Vide grille
      this.currentPlayer = "X"; // Reinitialise joueur
      this.winner = null; // Réinitialise gagnant
      this.$emit("gameReset"); // Émet un événement personnalisé
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
}
.game-info {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-size: 2rem;
  margin-bottom: 20px; /* Espace entre le conteneur info et le plateau de jeu */
}

.board {
  display: grid; /* Grille pour cellules */
  grid-template-columns: repeat(3, 1fr); /* 3 colonnes égales */
  gap: 5px; /* Espacement cellules */
  margin-bottom: 20px; /* Marge bas */
}

.reset {
  padding: 10px;
}

.cell {
  width: 100px; /* Largeur cellule */
  height: 100px; /* Hauteur cellule */
  display: flex; /* Affichage flexible */
  align-items: center; /* Centrage vertical */
  justify-content: center; /* Centrage horizontal */
  border: 2px solid plum; /* Bordure cellule */
  font-size: 4em; /* Taille symbole */
  cursor: pointer; /* Curseur pointeur */
}
</style>
