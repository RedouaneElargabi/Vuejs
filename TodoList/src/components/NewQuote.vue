<template>
    <div class="row">
        <form>
            <div class="col-sm-8 col-sm-offset-2 col-xs-12 col-md-6 col-md-offset-3 form-group">
                <label>Citation</label>
                <textarea class="form-control" rows="3" v-model="quote"></textarea>
            </div>
            <div class="col-sm-8 col-sm-offset-2 col-xs-12 col-md-6 col-md-offset-3 form-group">
               <button class="btn btn-primary" @click.prevent="createNew">Ajouter une citation</button>      <!-- le prevent sert a ne pas refresh la page -->
            </div>
        </form>
    </div>
</template>

<script>
    export default {
        data: function() {
            return {
                quote: ''     // Représente le quote du v-model (L.6), à chaque fois qu'on rentre un quote, il est save dans cette data
            };
        },
        methods: {
            createNew() {
                this.$emit('quoteAdded', this.quote);     // On emit la nouvelle citation créer
                this.quote = '';     // Permet de remettre le champ du form vide
            
            }
        }
    }
</script>

<style>

</style>


<!-- Ce component represente le formulaire dans lequel on ecrit une nouvelle citation et ou la nouvelle citation est envoyé a l'élément parent (App.vue)
        grâce à la fonction createNew et la méthode $emit-->