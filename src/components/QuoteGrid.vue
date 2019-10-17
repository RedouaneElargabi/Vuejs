<template>
    <div class="row">
        <!-- l'attribut native permet de rendre le composant cliquable -->
        <app-quote v-for="(quote, index) in quotes" @click.native="deleteQuote(index)">{{ quote }}</app-quote>
    </div>
</template>

<script>
    import Quote from './Quote.vue';   // On importe les citations

    export default {
        props: ['quotes'],

        components: {
            appQuote: Quote
        },

        methods: {
            deleteQuote(index) {
                this.$emit('quoteDeleted', index);
            }
        }
    }
</script>

<style>

</style>


<!-- Ce component représente l'ensemble des citations -->