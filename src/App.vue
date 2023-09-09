<template>
<v-app>
    <v-main>
        <PokemonSleepCookingTool />
    </v-main>
</v-app>
</template>

<script>
import PokemonSleepCookingTool from '@/components/PokemonSleepCookingTool.vue';

export default {
    data() {
        return {
        };
    },

    components: {PokemonSleepCookingTool},
};
</script>

<style scoped>
.v-application {
}
</style>
