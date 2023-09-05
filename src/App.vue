<template>
<v-app>
    <v-main>
        <!--
        <v-app-bar scroll-behavior="hide" density="compact" scroll-threshold="0">
            <v-app-bar-nav-icon></v-app-bar-nav-icon>

            <v-app-bar-title>Application</v-app-bar-title>
        </v-app-bar>
    -->
        <v-system-bar absolute>


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

export default {
    data() {
        return {
            version: VERSION,
        };
    },
    components: {PokemonSleepCookingTool},
};
</script>

<style scoped>
.v-application {
    font-family: "CJK Quote Font", "CJK Font";
}
</style>
