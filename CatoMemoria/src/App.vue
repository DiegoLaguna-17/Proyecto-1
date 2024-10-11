<template>
  <div id="app">
    <h1>Cato Memoria</h1>
    
    <!-- Contador de Puntos -->
    <div class="score-board">
      <p>Puntuación: {{ score }}</p>
      <p>Tiempo: {{ elapsedTime }} segundos</p> <!-- Mostrar el tiempo -->
      <p v-if="gameOver">¡Felicidades! Has completado el juego.</p>
    </div>
    
    <div class="board">
      <CardComponent
        v-for="(card, index) in cards"
        :key="index"
        :card="card"
        @flip-card="handleCardFlip"
      />
    </div>
    
    <button @click="resetGame" class="reset-button">Reiniciar Juego</button>
    
    
  </div>
</template>

<script>
//import de funcionalidades
import { ref,onMounted,onBeforeUnmount } from "vue";//importa funciones reactivas
import CardComponent from "./components/CardComponent.vue";//importa el componente de las tarjetas
//importar las imagenes de los lugares de la ucb la paz
import entrada from "./assets/images/entradaUCB.jpeg";
import bloquec from "./assets/images/bloquecUCB.jpeg";
import bloqued from "./assets/images/bloquedUCB.jpeg";
import biblioteca from "./assets/images/bibliotecaUCB.jpg";
import agora from "./assets/images/agoraUCB.jpeg";
import epc from "./assets/images/epcUCB.jpeg";
import cafeteria from "./assets/images/cafeteriaUCB.jpeg";
import arquitectura from "./assets/images/arquiUCB.jpg";
import coliseo from "./assets/images/coliseoUCB.jpeg";

// Definir imágenes
const images = [
  { id: 1, src: arquitectura, alt: 'Arquitectura' },
  { id: 2, src: biblioteca, alt: 'Biblioteca' },
  { id: 3, src: epc, alt: 'EPC' },
  { id: 4, src: cafeteria, alt: 'Cafeteria' },
  { id: 5, src: agora, alt: 'Ágora' },
  { id: 6, src: bloquec, alt: 'Bloque C' },
  { id: 7, src: bloqued, alt: 'Bloque D' },
  { id: 8, src: entrada, alt: 'Entrada' },
  { id: 9, src: coliseo, alt: 'Coliseo' },
];


export default {
  components: { CardComponent },
  setup() {

	// parte Adrian Ordoñez
	// Generar las cartas utilizando imágenes
  const generateCards = () => {
      const cards = [
        { id: 1, image: images[0], flipped: false, matched: false },
        { id: 1, image: images[0], flipped: false, matched: false },
        { id: 2, image: images[1], flipped: false, matched: false },
        { id: 2, image: images[1], flipped: false, matched: false },
        { id: 3, image: images[2], flipped: false, matched: false },
        { id: 3, image: images[2], flipped: false, matched: false },
        { id: 4, image: images[3], flipped: false, matched: false },
        { id: 4, image: images[3], flipped: false, matched: false },
        { id: 5, image: images[4], flipped: false, matched: false },
        { id: 5, image: images[4], flipped: false, matched: false },
        { id: 6, image: images[5], flipped: false, matched: false },
        { id: 6, image: images[5], flipped: false, matched: false },
        { id: 7, image: images[6], flipped: false, matched: false },
        { id: 7, image: images[6], flipped: false, matched: false },
        { id: 8, image: images[7], flipped: false, matched: false },
        { id: 8, image: images[7], flipped: false, matched: false },
        { id: 9, image: images[8], flipped: false, matched: false },
        { id: 9, image: images[8], flipped: false, matched: false },
      ].sort(() => Math.random() - 0.5); // Mezclar las cartas

      return cards;
    };

    const cards = ref(generateCards()); //inicia las cards de manera aleatoria
    const flippedCards = ref([]); //inicia y almacena en un array las cartas voleteadas
    const gameOver = ref(false); //inicia el gomeOver en falso para iniciar el juego
    
    // Nuevo estado para el puntaje
    const score = ref(0);
    //Inicio del tiempo de juego
    const elapsedTime = ref(0);
    let timerInterval;
    //inicia el contador de tiempo
    const startTimer = () => {
      timerInterval = setInterval(() => {
        elapsedTime.value++;
      }, 1000);
    };
    
    //para el contador de tiempo
    const stopTimer = () => {
      clearInterval(timerInterval);
    };

	  onMounted(() => {
      startTimer(); // Iniciar el temporizador cuando se monta el componente
    });

    onBeforeUnmount(() => {
      stopTimer(); // Detener el temporizador cuando se desmonta el componente
    });



	// Manejar el volteo de una carta
    const handleCardFlip = (card) => {
      if (flippedCards.value.length < 2 && !card.flipped && !card.matched) {
        card.flipped = true;
        flippedCards.value.push(card);

        if (flippedCards.value.length === 2) {
          // Verificar si las cartas coinciden
          setTimeout(checkMatch, 1000);
        }
      }
    };

    // Verificar si las cartas volteadas coinciden
    const checkMatch = () => {
      const [card1, card2] = flippedCards.value;
      if (card1.id === card2.id) {
        card1.matched = true;
        card2.matched = true;
        // Incrementar el puntaje en 10 puntos por par
        score.value += 10;
      } else {
        card1.flipped = false;
        card2.flipped = false;
      }
      //limpia las cartas volteadas tras verificar pares
      flippedCards.value = [];

      // Verificar si el juego ha terminado
      if (cards.value.every((card) => card.matched)) {
        gameOver.value = true;
        stopTimer();  
        console.log("Game Over!");
      }
    };

    // Reiniciar el juego
    const resetGame = () => {
      stopTimer();
      cards.value = generateCards();
      flippedCards.value = [];
      gameOver.value = false;
      score.value = 0; // Reiniciar el puntaje
      elapsedTime.value = 0; // Reiniciar el tiempo
      startTimer();
    };

    return { cards, handleCardFlip, resetGame, gameOver, score,elapsedTime };
		

  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  margin-top: 50px;
}

.score-board {
  margin-bottom: 20px;
  font-size: 24px;
  font-weight: bold;
  color: white; 
}

.board {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  max-width: 100%;
  margin: 0 auto;
}

.reset-button {
  margin-top: 20px;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}
</style>

