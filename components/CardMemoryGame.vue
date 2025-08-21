<script>
import img1 from "@/assets/images/logo/genie-mecanique.png";
import img2 from "@/assets/images/logo/la-cooperation.png";
import img3 from "@/assets/images/logo/papier-carton.png";
import img4 from "@/assets/images/logo/partager.png";
import img5 from "@/assets/images/logo/peinture-au-rouleau.png";
import img6 from "@/assets/images/logo/sacs-a-provisions.png";
import img7 from "@/assets/images/logo/stethoscope.png";
import img8 from "@/assets/images/logo/terminal.png";

export default {
    name: "CardMemory",
    data() {
        return {
            pictures: [img1, img2, img3, img4, img5, img6, img7, img8],
            pairsPictures: [],
            flippedCards: [], // cartes actuellement retournées
            lockBoard: false, // empêche de cliquer pendant la vérification
        };
    },
    methods: {
        mix() {
            const createPairs = [];
            for (let i = 0; i < this.pictures.length; i++) {
                createPairs.push(
                    {
                        id: i,
                        img: this.pictures[i],
                        flipped: false,
                        matched: false,
                    },
                    {
                        id: i,
                        img: this.pictures[i],
                        flipped: false,
                        matched: false,
                    }
                );
            }
            this.pairsPictures = createPairs.sort(() => Math.random() - 0.5);
        },

        flipCard(card) {
            if (this.lockBoard) return; // bloque si on attend la fin d'une vérif
            if (card.flipped || card.matched) return; // ignore si déjà retournée ou trouvée

            card.flipped = true;
            this.flippedCards.push(card);

            if (this.flippedCards.length === 2) {
                this.checkForMatch();
            }
        },

        checkForMatch() {
            this.lockBoard = true;
            const [card1, card2] = this.flippedCards;

            if (card1.id === card2.id) {
                // Paire trouvée
                card1.matched = true;
                card2.matched = true;
                this.resetTurn();
            } else {
                // Pas de match → on retourne après un délai
                setTimeout(() => {
                    card1.flipped = false;
                    card2.flipped = false;
                    this.resetTurn();
                }, 1000);
            }
        },

        resetTurn() {
            this.flippedCards = [];
            this.lockBoard = false;
        },
    },
    mounted() {
        this.mix();
    },
};
</script>

<template>
    <div class="container flex justify-center mx-auto my-12">
        <div class="grid grid-cols-2  sm:grid-cols-4 gap-12">
            <div
                v-for="(card, index) in pairsPictures"
                :key="`${card.id}-${index}`"
                class="card"
                :class="{ flipped: card.flipped || card.matched }"
                @click="flipCard(card)"
            >
                <div class="card-inner">
                    <div class="card-front">
                        <!-- Dos de la carte -->
                    </div>
                    <div class="card-back flex items-center justify-center">
                        <img
                            :src="card.img"
                            :alt="`carte ${index}`"
                            class="w-1/3 flex justify-center items-center h-auto max-h-full object-contain"
                        />
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style>
  .card {
    perspective: 1000px;
    width: 100px;
    height: 100px;
    cursor: pointer;
  }

  .card-inner {
    position: relative;
    width: 100%;
    height: 100%;
    transition: transform 0.6s;
    transform-style: preserve-3d;
  }

  .card.flipped .card-inner {
    transform: rotateY(180deg);
  }

  .card-front,
  .card-back {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    border: 1px solid #ccc;
    border-radius: 8px;
  }

  .card-front {
    background: #f5f5f5;
  }

  .card-back {
    background: white;
    transform: rotateY(180deg);
  }
</style>
