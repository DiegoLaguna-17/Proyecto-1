<template>
  <div class="card" :class="{ flipped: card.flipped || card.matched }" @click="flipCard">
    <div class="front">
      <!-- Mostrar el logoUcb.png en el frente de la carta -->
      <img src="../assets/images/logoUcb.png" alt="Card front" class="logo" />
    </div>
    <div class="back">
      <!-- Mostrar la imagen del lugar en la parte trasera de la carta -->
      <img :src="card.image.src" :alt="card.image.alt" class="photo-image" />
    </div>
  </div>
</template><script>
export default {
  name: "CardComponent",
  props: {
    card: Object,
  },
  methods: {
    //verificar si la carta no tiene par y no volteada
    flipCard() {//metodo ´para voltear la carta
      if (!this.card.flipped && !this.card.matched) {
        //evento indicador de que la carta fue volteada
        this.$emit("flip-card", this.card);
      }
    },
  },
};
</script>

<style scoped>
.card {
  width: 100px; /* Ajusta el tamaño según tus necesidades */
  height: 100px;
  display: inline-block;
  perspective: 1000px; /* Efecto 3D */
  cursor: pointer;
  position: relative; /* Necesario para posicionar las caras */
  margin: 10px; /* Espaciado entre cartas */
  border: 1px solid #ccc; /* Borde de la carta */
  border-radius: 8px; /* Bordes redondeados */
  background-color: #f9f9f9; /* Color de fondo */
}

.front,
.back {
  width: 100%;
  height: 100%;
  position: absolute;
  backface-visibility: hidden; /* Esconde la cara opuesta */
  transition: transform 0.5s; /* Transición al voltear */
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 8px;
}

.front {
  top:1px;
  right: 1px;
  transform: rotateY(0deg); /* Frente normal */
}

.back {
  right: 0.2px;
  top:0.5px;
  transform: rotateY(-180deg); /* Espalda normal */
}

.card.flipped .front {
  transform: rotateY(180deg); /* Volteo de la carta */
}

.card.flipped .back {
  transform: rotateY(0deg); /* Mostrar la parte trasera */
}

.logo {
  width: 100%; /* Ajustar imagen al ancho de la carta */
  height: 100%; /* Ajustar imagen al alto de la carta */
  object-fit: cover; /* Cubrir el área de la carta manteniendo la relación de aspecto */
  border-radius: 8px;
}

.photo-image {
  width: 100%; /* Ajustar imagen al ancho de la carta */
  height: 100%; /* Ajustar imagen al alto de la carta */
  object-fit: cover; /* Cubrir el área de la carta manteniendo la relación de aspecto */
  border-radius: 8px;
}
</style>