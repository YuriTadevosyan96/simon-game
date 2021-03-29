<template>
    <div class="game">
        <h2 class="text-center">Simon Says</h2>
        <div class="game-data">
            <template v-if="gameLost">
                <h2>Sorry you lost after {{ gameRound }} rounds</h2>
            </template>
            <div v-if="!gameStarted" class="row mb-1 justify-content-between">
                <GameMode :selectedGameMode="selectedGameMode" @changeGameMode="getAndSetGameMode" />
            </div>
            <div class="mb-1 text-center">
                Round: <b>{{ gameRound }}</b>
            </div>
            <div v-if="!gameStarted" class="flex-center d-flex mb-1">
                <button class="btn btn-primary" @click="startGame">Start</button>
            </div>
        </div>
        <div class="game-area">
            <div class="row flex-center">
                <Quorter data-id="1" id="_1quorter" topLeft :gameLost="gameLost" />
                <Quorter data-id="2" id="_2quorter" topRight :gameLost="gameLost" />
            </div>
            <div class="row flex-center">
                <Quorter data-id="4" id="_4quorter" bottomLeft :gameLost="gameLost" />
                <Quorter data-id="3" id="_3quorter" bottomRight :gameLost="gameLost" />
            </div>
        </div>

        <audio id="audio-1">
            <source src="./sounds/1.mp3" type="audio/mpeg" />
        </audio>
        <audio id="audio-2">
            <source src="./sounds/2.mp3" type="audio/mpeg" />
        </audio>
        <audio id="audio-3">
            <source src="./sounds/3.mp3" type="audio/mpeg" />
        </audio>
        <audio id="audio-4">
            <source src="./sounds/4.mp3" type="audio/mpeg" />
        </audio>
    </div>
</template>

<script>
import Quorter from "./components/Quorter.vue";
import GameMode from "./components/GameMode.vue";

export default {
    name: "App",
    components: {
        Quorter,
        GameMode,
    },

    data() {
        return {
            gameLost: false,
            gameStarted: false,
            clickDisabledMode: false, //will not allow clicks if sequence is showing
            generatedButtonSequence: [],
            gameRound: 0,
            timerCallCount: 0,
            clickCounter: 0,
            selectedGameMode: "easy", //easy mode by default
            gameOptions: {
                easy: {
                    timeInterval: 1500, //milliseconds
                },
                normal: {
                    timeInterval: 1000, //milliseconds
                },
                hard: {
                    timeInterval: 400, //milliseconds
                },
            },
        };
    },

    methods: {
        generateSequence() {
            const mode = this.selectedGameMode;
            const timeInterval = this.gameOptions[mode].timeInterval;
            this.clickDisabledMode = true;
            this.timerCallCount = 0;
            this.clickCounter = 0;
            this.gameRound++;
            const quorter = Math.ceil(Math.random() * 4);
            this.generatedButtonSequence.push(quorter);

            //calls timeout recursively untill round number is reached
            const timeout = () => {
                if (this.timerCallCount == this.generatedButtonSequence.length) {
                    this.clickDisabledMode = false;
                    return;
                }

                this.blinkWithSoundEffect(this.generatedButtonSequence[this.timerCallCount]);
                this.timerCallCount++;

                setTimeout(timeout, timeInterval);
            };

            setTimeout(timeout, timeInterval);
        },

        //check clicked order rightness
        checkSequence(id, clickCount) {
            const index = clickCount - 1;
            if (this.generatedButtonSequence[index] != id) {
                this.gameLost = true;
                this.gameStarted = false;
                this.clickDisabledMode = true;
                return;
            }

            if (this.generatedButtonSequence.length == clickCount) {
                const mode = this.selectedGameMode;
                const timeInterval = this.gameOptions[mode].timeInterval;
                this.clickDisabledMode = true;
                setTimeout(this.generateSequence, timeInterval);
            }
        },

        resetGame() {
            this.gameLost = false;
            this.generatedButtonSequence = [];
            this.gameRound = 0;
            this.clickDisabledMode = false;
            this.timerCallCount = 0;
            this.clickCounter = 0;
        },

        startGame() {
            this.gameStarted = true;
            this.resetGame();
            this.generateSequence();
        },

        blinkWithSoundEffect(id) {
            document.getElementById(`audio-${id}`).play();
            document.querySelectorAll(".quorter").forEach((item) => {
                item.classList.remove("active");
            });
            document.querySelector(`#_${id}quorter`).classList.add("active");
            setTimeout(() => {
                document.querySelector(`#_${id}quorter`).classList.remove("active");
            }, 100);
        },

        getAndSetGameMode(mode) {
            this.selectedGameMode = mode;
        },

        quorterClickHandler(event) {
            //dont allow clicks in sequence showing stage
            if (this.clickDisabledMode) {
                return;
            }

            const id = event.target.dataset.id;
            this.blinkWithSoundEffect(id);
            this.clickCounter += 1;

            this.checkSequence(id, this.clickCounter);
        },
    },

    mounted() {
        document.querySelectorAll(".quorter").forEach((item) => {
            item.addEventListener("click", this.quorterClickHandler);
        });
    },
};
</script>

<style>
#app {
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
}

.game {
    min-height: 350px;
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
}
</style>
