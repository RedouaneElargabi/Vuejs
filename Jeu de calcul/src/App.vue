<template>
    <div class="container">
        <div class="row">
            <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
                <h1>Animations</h1>
            <hr>
                <select v-model="animations" class="form-control">
                    <option value="fade">Fade</option>
                    <option value="slide">Slide</option>
                </select>
                <br><br>
                <button class="btn btn-primary" @click="show = !show">Afficher les Blocs</button>
                <br><br>
                <transition :name="animations">
                    <div class="alert alert-info" v-if="show">Bloc 1</div>
                </transition> 
                <transition  appear
                    enter-active-class="animated bounce"         
                    leave-active-class="animated shake">                           
                    <div class="alert alert-info" v-if="show">Bloc 2</div>
                </transition> 
                <transition  :name="animations" mode="out-in">   <!-- Mode out-in permet que l'élément actuel sort en premier, puis, une fois terminé, le nouvel élément entre en  -->
                    <div class="alert alert-info" v-if="show" key="info">Bloc 3</div> <!-- Lors d'une transition entre 2 éléments portant la meme balise, il faut utiliser key pour les distinguer avec des attributs uniques  -->
                    <div class="alert alert-warning" v-else key="warning">Bloc 4</div>
                </transition>
            <hr>
                <button class="btn btn-primary" @click="load = !load">Chargé / Retirer Element </button>
                <hr>
                <transition
                    @before-enter="beforeEnter"
                    @enter="enter"
                    @after-enter="afterEnter"
                    @enter-cancelled="enterCancelled"

                    @before-leave="beforeLeave"
                    @leave="leave"
                    @after-leave="afterLeave"
                    @leave-cancelled="leaveCancelled"
                    :css="false">                       <!-- Appliquer uniquement lors d'une transition Js, pour ignorer la détection CSS et donc ses régles -->
                    <div style="width: 300px; height: 100px; background-color: lightgreen" v-if="load"></div>
                </transition>
            <hr>
                <button 
                    class="btn btn-primary" 
                    @click="selectedComponent == 'app-success-alert' ? selectedComponent = 'app-danger-alert' : selectedComponent = 'app-success-alert'">Changer le composant</button>
                <br><br>
                <transition mode="out-in" name="fade">
                    <component :is="selectedComponent"></component>
                </transition>
            <hr>
                <button class="btn btn-primary" @click="addItem">Ajouter</button>
                <br><br>
                <ul class="list-group">
                    <transition-group name="slide">                         <!--    Pour le transition-group, toujours mettre :key -->
                        <li 
                        class="list-group-item" v-for="(number, index) in numbers"          
                        @click="removeItem(index)" 
                        style="cursor: pointer"
                        :key="number">{{ number }}
                    </li>
                    </transition-group>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
    import DangerAlert from './DangerAlert.vue';
    import SuccessAlert from './SuccessAlert.vue';

    export default {
        data() {
            return {
                show: false,
                load: true,
                animations: 'fade',
                elementWidth: 100,
                selectedComponent: 'app-success-alert',
                numbers: [1, 2, 3, 4, 5]
            }
        },
        methods: {
            beforeEnter(el) {
                console.log('beforeEnter');
                this.elementWidth = 100;
                el.style.width = this.elementWidth + 'px';
            },
            enter(el, done){                          // La méthode enter() est là où le code Js est vraiment joué.
                console.log('enter');
                let round = 1;
                const interval = setInterval(() => {
                    el.style.width = (this.elementWidth + round * 10) + 'px';
                    round++;
                    if (round > 20) {
                        clearInterval(interval);
                        done();                   // La fonction done() permet de dire a Vuejs que le code est fini. En Css, il ne sert a rien car Vuejs voit dans le code la configuration du Css.
                    }
                }, 20);
            },
            afterEnter(el) {
                console.log('afterEnter');
            },
            enterCancelled(el) {                      // La méthode enterCancelled() est là où l'animation est annulé.
                console.log('enterCancelled');
            },
            beforeLeave(el) {
                console.log('beforeLeave');
                this.elementWidth = 300;
                el.style.width = this.elementWidth + 'px';
            },
            leave(el, done) {                        // La méthode leave() est là où nous supprimons ce que l'on souhaite une fois le code finit
                console.log('leave');
                let round = 1;
                const interval = setInterval(() => {
                el.style.width = (this.elementWidth - round * 10) + 'px';
                    round++;
                    if (round > 20) {
                        clearInterval(interval);
                        done();
                    }
                }, 20);        
            },
            afterLeave(el) {
                console.log('afterLeave');
            },
            leaveCancelled(el) {
                console.log('leaveCancelled');
            },
            addItem() {
                const pos = Math.floor(Math.random() * this.numbers.length);
                this.numbers.splice(pos, 0, this.numbers.length + 1);
            },
            removeItem(index) {
                this.numbers.splice(index, 1);
            }                                    
        },
        components: {
            appDangerAlert: DangerAlert,
            appSuccessAlert: SuccessAlert
        }
    }
</script>

<style>

/* Css pour la transition */
    .fade-enter {
        opacity: 0;
    }

    .fade-enter-active {
        transition: opacity 1s;
    }

    .fade-leave {
       /* opacity: 1; */
    }

    .fade-leave-active {
        transition: opacity 1s;
        opacity: 0;
    }


/* Css pour l'animations */
    .slide-enter {
        opacity: 0;
    }

    .slide-enter-active {
        animation: slide-in 1s ease-out forwards;
        transition: opacity .5s;
    }

    .slide-leave {
        /* ... */
    }

    .slide-leave-active {
        animation: slide-out 1s ease-out forwards;
        transition: opacity 1s;
        opacity: 0;
        position: absolute;                 /* Permet d'eviter le saut lorsqu'un élément est supprimé */
    }

    .slide-move {                            /* Spécifique à transition-group, qui permet d'animer le changement de position d'un element */
        transition: transform 1s;
    }

    @keyframes slide-in {
        from {
            transform: translateY(20px);
        }
        to {
            transform: translateY(0);
        }
    }

    @keyframes slide-out {
        from {
            transform: translateY(0);
        }
        to {
            transform: translateY(20px);
        }
    }

</style>

<!-- Pour créer un animation, il faut qu'elle soit entre les balise transition dans le template, et il faut écrire du CSS 
    Il Existe deux type : animation ou transition 

 Appear permet d'effectuer l'animation au chargement de la page  

 ligne 17 à 21, c'est la méthode pour utiliser d'autre classe d'animation importer (ex: libraire externe) 

    Il est possible d'attribuer dynamiquement les classes comme pour le bloc 1
    
-->