<template>
<div>

    <v-avatar size="3.5em">
        <v-img :src="'image/' + recipe.category + '/' + recipe.lowercase + '.png'" alt="recipe.name.lowercase"></v-img>
    </v-avatar>

    <span :style="style">
        {{ recipe.name }} &lt;{{ recipe.size }}&gt;
    </span>

</div>
</template>

<script>
export default {
    props: [
        'recipe',
        'collected',
    ],
    data() {
        return {
            style: {},
        };
    },
    methods: {
        whoami() {
        },
        getRecipeColor() {
            let able = this.recipe.able;
            if (able === 1) {
                this.style.color = 'green';
            } else {
                if (this.collected) {
                    this.style.color = null;
                } else {
                    this.style.color = 'red';
                }
            }
        },
    },
    beforeMount() {
    },
    mounted() {
        this.getRecipeColor();
    },
    watch: {
        'recipe.able': function(new_val, old_val) {
            this.getRecipeColor();
        },
        'collected': function(new_val, old_val) {
            this.getRecipeColor();
        },
    },
};
</script>

<style>
</style>
