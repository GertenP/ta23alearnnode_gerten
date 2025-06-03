<script setup>
import { computed, onUnmounted, ref } from 'vue';

let cookies = ref(0);

let buildings = ref([
    { name: 'Cursor', price: 15, count: 0, cps: 0.1 },
    { name: 'ASP NET', price: 30, count: 0, cps: 0.5 },
    { name: 'Grandma', price: 100, count: 0, cps: 1 },
    { name: 'Farm', price: 1000, count: 0, cps: 10 },
    { name: 'Factory', price: 10000, count: 0, cps: 100 },
]);

let cps = computed(() => {
    return buildings.value.reduce((total, building) => total + building.cps * building.count, 0);
});

const cpsInterval = setInterval(() => {
    cookies.value += cps.value;
    document.title = `🍪 ${cookies.value} cookies`;
}, 1000);

let eliitHakkerVisible = ref(false);

function buyBuilding(building) {
    if (cookies.value >= building.price) {
        cookies.value -= building.price;
        building.price += Math.ceil(building.price * 0.15);
        building.count++;

        if (building.name === 'ASP NET') {
            eliitHakkerVisible.value = true;
            setTimeout(() => {
                eliitHakkerVisible.value = false;
            }, 2000);
        }
    }
}

onUnmounted(() => {
    clearInterval(cpsInterval);
});
</script>

<template>
    <div class="columns">
        <div class="column is-3 has-background-primary has-text-centered">
            <h1 class="is-size-1"><b>Cookies: {{ +cookies.toFixed(1) }}</b></h1>
            <h3 class="is-size-3"><b>CPS: {{ +cps.toFixed(1) }}</b></h3>
            <figure class="image is-square m-5">
                <img
                    @click="cookies++"
                    class="is-rounded"
                    src="https://www.bcskoolitus.ee/wp-content/uploads/sites/2/2022/08/lektor_gunnar-4.jpg"
                    alt=""
                />
            </figure>
        </div>
        <div class="column is-7 has-background-info">Second column</div>
        <div class="column is-2 has-background-warning">
            <button
                class="button is-large is-fullwidth is-primary mb-2"
                v-for="building in buildings"
                :key="building.name"
                :disabled="cookies < building.price"
                @click="buyBuilding(building)"
            >
                {{ building.name }} 🍪{{ building.price }} #{{ building.count }}
            </button>
        </div>
    </div>

    <div
        v-if="eliitHakkerVisible"
        class="notification is-danger has-text-centered is-size-1"
        style="position: fixed; top: 40%; left: 50%; transform: translate(-50%, -50%); z-index: 9999;"
    >
        ELIITHÄKKER
    </div>
</template>
