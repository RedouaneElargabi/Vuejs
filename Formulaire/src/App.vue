<template>
    <div class="container">
        <form v-if="!submitted">
            <div class="row">
                <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
                    <app-full-name v-model="fullName"></app-full-name>
                    <div class="form-group">
                        <label>Mail</label>
                        <input type="text" class="form-control" v-model="email">
                    </div>
                    <div class="form-group">
                        <label>Mot de Passe</label>
                        <input type="password" class="form-control" v-model="password">
                    </div>
                    <div class="form-group">
                        <label>
                            Sauvegarder ? <br>
                            <input type="radio" value="Oui" v-model="storeData"> Oui
                        </label>
                        <label>
                            <input type="radio" value="Non" v-model="storeData"> Non
                        </label>
                    </div>
                    <button
                            type="submit"
                            @click.prevent="submitted = true"
                            class="btn btn-primary">Soumettre</button>
                </div>
            </div>
        </form>
        <hr>
        <div class="row" v-if="submitted">
            <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
                <div class="panel panel-default">
                    <div class="panel-heading">
                        <h4>Vos Données</h4>
                    </div>
                    <div class="panel-body">
                        <p>Nom complet : {{ fullName }}</p>
                        <p>Mail: {{ email }}</p>
                        <p>Mot de passe : {{ password }}</p>
                        <p>Sauvegarder ? : {{ storeData }}</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import FullName from './FullName.vue';

    export default {
        data() {
            return {
                fullName: 'Toto Redouane',
                email: '',
                password: '',
                storeData: 'Yes',
                submitted: false
            }
        },
        components: {
            appFullName : FullName
        }
    }
</script>

<style>
</style>
