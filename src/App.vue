<template>
  <body id="app">
    <!-- Fond vidéo -->
    <div class="video-background">
      <video ref="videoPlayer" loop>
        <!-- Source de la vidéo -->
        <source src="/video0.mp4" type="video/mp4" />
      </video>
      <!-- Btn Play, affiché si showButton est vrai -->
      <button
        v-if="showButton && !isGameActive"
        @click="playVideo"
        class="play-button"
      >
        OK Let's go
      </button>
      <!-- Btn Jeu, affiché si showGameButton est vrai -->
      <button
        v-if="showGameButton && !isGameActive"
        @click="showGame"
        class="game-button"
      >
        une petite game ?!
      </button>
      <!-- Overlay de jeu, affiché si showTicTacToe est vrai -->
      <div v-if="showTicTacToe" class="game-overlay">
        <AppMorpion />
      </div>
    </div>
    <!-- Contrôle volume -->
    <div class="volume-control">
      <input
        type="range"
        min="0"
        max="100"
        value="100"
        @input="setVolume"
        orient="vertical"
      />
    </div>
    <button
      v-if="showChangeBackgroundButton"
      @click="changeBackground"
      class="change-background-button"
    >
      Changer le fond
    </button>
  </body>
</template>

<script>
import AppMorpion from "./components/AppMorpion.vue"; // Importe composant Morpion

export default {
  name: "App", // Nom du composant
  components: {
    AppMorpion, // Déclare sous-composant Morpion
  },
  data() {
    // Données du composant
    return {
      showButton: true, // Etat btn Play
      showGameButton: false, // Etat btn Jeu
      showTicTacToe: false, // Affichage jeu Morpion
      showChangeBackgroundButton: true, // Etat du bouton de changement de fond
      isGameActive: false, // Nouvelle propriété pour suivre l'état du jeu
    };
  },
  methods: {
    playVideo() {
      // Joue vidéo et MAJ états btns
      this.$refs.videoPlayer.play();
      this.showButton = false;
      this.showGameButton = true;
    },
    showGame() {
      this.showTicTacToe = true;
      this.showGameButton = false;
      this.isGameActive = true; // Met à jour l'état du jeu
    },
    setVolume(event) {
      // Règle volume vidéo
      const volume = event.target.value / 100;
      this.$refs.videoPlayer.volume = volume;
    },
    changeBackground() {
      const videoNumber = Math.floor(Math.random() * 7);
      this.$refs.videoPlayer.src = `/video${videoNumber}.mp4`;
      this.$refs.videoPlayer.load();
      this.playVideo();
    },
  },
};
</script>

<style>
body {
  /* Corps sans marges */
  margin: 0;
  padding: 0;
}

.change-background-button {
  /* Style du bouton de changement de fond */
  position: absolute;
  top: 90%;
  left: 50%;
  transform: translate(-50%, -50%);
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}
.video-background {
  /* Conteneur vidéo */
  position: relative;
  width: 100%;
  height: 100vh;
  overflow: hidden;
}
.video-background video {
  /* Vidéo centrée */
  position: absolute;
  top: 50%;
  left: 50%;
  min-width: 100%;
  min-height: 100%;
  transform: translate(-50%, -50%);
}
.game-overlay {
  /* Overlay jeu centré */
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 10; /* Au-dessus vidéo */
}
.play-button {
  /* Style btn Play */
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}
.game-button {
  /* Style btn Jeu */
  position: absolute;
  top: 60%;
  left: 50%;
  transform: translate(-50%, -50%);
}
button {
  cursor: pointer;
  padding: 1%;
  color: white;
  background-color: transparent; /* Fond transparent */
  border: 1px solid #ccc; /* Optionnel : ajoute une bordure légère */
  transition: background-color 0.3s; /* Effet de transition pour le survol */
} /* Style au survol des boutons */
button:hover {
  background-color: plum; /* Change le fond au survol */
}
.volume-control {
  /* Contrôle volume */
  position: absolute;
  left: 10px;
  top: 50%;
  transform: translateY(-50%);
}

.volume-control input[type="range"] {
  /* Style barre volume */
  writing-mode: bt-lr;
  -webkit-appearance: slider-vertical;
  width: 8px;
  height: 200px;
  padding: 0 5px;
}
</style>
