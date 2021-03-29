<template>
    <div :class="styling" class="quorter"></div>
</template>

<script>
export default {
    name: "Quorter",
    props: {
        topLeft: Boolean,
        topRight: Boolean,
        bottomLeft: Boolean,
        bottomRight: Boolean,
        activeItem: Boolean,
        gameLost: Boolean,
    },

    data() {
        return {
            styling: {
                "top-left": this.topLeft,
                "top-right": this.topRight,
                "bottom-left": this.bottomLeft,
                "bottom-right": this.bottomRight,
                active: this.activeItem,
                disabled: this.gameLost,
            },
        };
    },
};
</script>

<style scoped>
.quorter {
    width: 100px;
    height: 100px;
    cursor: pointer;
}

.quorter.disabled {
    cursor: auto;
}

.top-left {
    background-color: rgba(240, 128, 128, 0.5);
    border-top-left-radius: 100%;
    border-top: 2px solid transparent;
    border-left: 2px solid transparent;
}

.top-left.active {
    background-color: rgba(240, 128, 128, 1);
}

.top-left:hover {
    border-top: 2px solid black;
    border-left: 2px solid black;
}

.top-right {
    background-color: rgba(255, 165, 0, 0.5);
    border-top-right-radius: 100%;
    border-top: 2px solid transparent;
    border-right: 2px solid transparent;
}

.top-right.active {
    background-color: rgba(255, 165, 1);
}

.top-right:hover {
    border-top: 2px solid black;
    border-right: 2px solid black;
}

.bottom-left {
    background-color: rgba(135, 206, 250, 0.5);
    border-bottom-left-radius: 100%;
    border-left: 2px solid transparent;
    border-bottom: 2px solid transparent;
}

.bottom-left.active {
    background-color: rgba(135, 206, 250, 1);
}

.bottom-left:hover {
    border-left: 2px solid black;
    border-bottom: 2px solid black;
}

.bottom-right {
    background-color: rgba(32, 178, 170, 0.5);
    border-bottom-right-radius: 100%;
    border-right: 2px solid transparent;
    border-bottom: 2px solid transparent;
}

.bottom-right.active {
    background-color: rgba(32, 178, 170, 1);
}

.bottom-right:hover {
    border-right: 2px solid black;
    border-bottom: 2px solid black;
}
</style>
