<template>
<v-app>
    <v-main>

        <v-system-bar absolute>
            <a :href="homepage" target="_blank" rel="noopener">Homepage</a>
            <span class="ms-2">v{{ version }}</span>
        </v-system-bar>

        <PokemonSleepCookingTool />
    </v-main>
</v-app>
</template>

<script>
import PokemonSleepCookingTool from '@/components/PokemonSleepCookingTool.vue';
import config from '../package.json';
const VERSION = config.version;
const HOMEPAGE = config.homepage;

export default {
    data() {
        return {
            homepage: HOMEPAGE,
            version: VERSION,
        };
    },

    components: {PokemonSleepCookingTool},
};
</script>

<style scoped>
.v-application {
}
</style>
