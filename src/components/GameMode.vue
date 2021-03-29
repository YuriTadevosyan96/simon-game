<template>
    <div class="form-group">
        <label for="easy">Easy</label>
        <input type="radio" name="game-mode" @change="changeGameMode" value="easy" id="easy" :checked="selectedGameMode == 'easy'" />
    </div>
    <div class="form-group">
        <label for="normal">Normal</label>
        <input type="radio" name="game-mode" @change="changeGameMode" value="normal" id="normal" :checked="selectedGameMode == 'normal'" />
    </div>
    <div class="form-group">
        <label for="hard">Hard</label>
        <input type="radio" name="game-mode" @change="changeGameMode" value="hard" id="hard" :checked="selectedGameMode == 'hard'" />
    </div>
</template>

<script>
export default {
    name: "GameMode",
    emits: ["changeGameMode"],
    props: ["selectedGameMode"],
    methods: {
        changeGameMode(event) {
            this.$emit("changeGameMode", event.target.value);
        },
    },
};
</script>

<style scoped>
.form-group * {
    cursor: pointer;
}
</style>
