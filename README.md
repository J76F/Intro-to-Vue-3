# Intro-to-Vue-3
Code for Vue Mastery's Intro to Vue 3 course
bron: https://www.vuemastery.com/courses/intro-to-vue-3/intro-to-vue3/
Cursus Vue.js v3 intro
Oefenlocatie
--------------------------
In index.html
<!-- Import Vue.js -->
<script src="https://unpkg.com/vue@3"></script>
<!-- Locatie App -->
<div id="app">
    <!-- Verder opmaak, posities -->
</div>
<!-- Import App -->
<script src="./main.js"></script>
<!-- Mount App -->
<script>
    // positie waar app werkt.
    const mountedApp = app.mount('#app')
</script>
In main.js
    const app = Vue.createApp({
        // Variabelen, Variabele array's
        data() {
            return {
                cart:0,
                product: 'Socks',
                details: ['50% cotton', '30% wool', '20% polyester'],
                selectedVariant: 0,
                variants: [
                    { id: 2234, color: 'green', image: './assets/images/socks_green.jpg' },
                    { id: 2235, color: 'blue', image: './assets/images/socks_blue.jpg' },
                    ],
                styles: {
                    color: 'red',
                    fontSize: '14px'
                }
            }
        },
        // Functies
        methods: {
            addToCart() {
                this.cart += 1
            },
            updateVariant(index) {
                this.selectedVariant = index
            },
        },
        // berekeningen op waarden, op te vragen als waarde
        //<!-- Computed Properties -->
        computed: { 
            title() {
                return this.brand + ' ' + this.product
            },
            image() {
                return this.variants[this.selectedVariant].image
            },
        }
    })
--------------------------
<!-- Verder opmaak, posities -->
    <!-- Waarde ophalen en weergeven -->
    {{ JavaScript-waarde }}
    <!--    
        bijv.: variabele -> waarde wordt weergegeven op positie
        Maar je kan ook bewerkingen doen waar dan de uitkomst van wordt weergegeven 
    -->
    <!-- voorbeeld -->
        {{ product }} //geeft de waarde van product weer
        {{ brand + ' ' + product }} //geeft de waarde van brand spatie product
    <!-- Attribute Binding (waarde ophalen) -->
    v-bind:attr="JavaScript-waarde"  --> Afgekort: :attr="JavaScript-waarde"
    <!-- voorbeeld: -->
        <!-- src attribute, verbinden met  variabele "image" data -->
        <img v-bind:src="image"></img> of <img :src="image"></img> 
        <!-- href attribute, verbinden met  variabele "url" data -->
        <a v-bind:href="url">Link</a> of <a :href="url">Link</a> 
    <!-- Conditional Rendering -->
    <!-- Wel/Niet Renderen -->
        v-if="JavaScript-Voorwaarde"
        v-else-if="JavaScript-Voorwaarde"
        v-else
        <!-- voorbeeld: [Alleen wat waar is wordt uitgevoerd]-->
            <!-- object weergeven(toepssen), afhankelijk voorwaarde met  variabele "voorwaarde" data -->
            <p v-if="variabele > 10">Waar</p>
            <p v-else>Nietwaar</p>
    <!-- Weergeven/Verbergen -->
    v-show="JavaScript-Voorwaarde"
    <!-- voorbeeld: [Waat waar is wordt uitgevoerd, niet waar krijgt de style "style=display: none;"] mee zodat het niet weergegeven wordt, maar wel in de uitvoer staat-->
    <!-- Lijst maken (loop) --> 
    v-for="variabele in waarde-array" --> variabele te gebruiken binnen de syntax
    <!-- voorbeeld: -->
        <!-- syntax wordt ..x toegepast voor elke loop in v-for -->
        <ul>
            <li v-for="detail in details">{{ detail }}</li>
        </ul>
        <!-- gelijk principe, maar bind gelijk de v-bind:key aan de variant.id ; voor later makelijk hergebuik-->
        <div v-for="variant in variants" :key="variant.id">{{ variant.color }}</div>
        <!-- en nu met teller (index)-->
        <div v-for="(variant, index) in variants" :key="variant.id" @mouseover="updateVariant(index)">{{ variant.color }}</div>
    <!-- Event Handling -->
    v-on:click="javascript-functie-app"  of @click="javascript-functie-app" 
    @mouseover=
    <!-- -> javascript-functie-app toevoegen "methods" aan de app data welke uitgevoerd moet worden. -->
    <!-- voorbeeld: -->
        <button class="button" v-on:click="addToCart">Add to Cart</button>
        <button class="button" @click="addToCart">Add to Cart</button>
        <div v-for="variant in variants" :key="variant.id" @mouseover="updateImage(variant.image)">{{ variant.color }}</div>
    <!-- Class & Style Binding -->
    :style="{ CSS_property: JavaScript-Waarde }" //losse waarde
    :style="{ JavaScript-Waarde }"  //style object, onder app.data
    :class="{ CSS_Class: JavaScript-voorwaarde }" // CSS_Class wordt 
    toegepast wanneer voorwaarde voldoet (toegevoegd aan eventueel een bestaande class).
    :class="[JavaScript-voorwaarde ? CSS_Class : CSS_Class]" // voorwaarde, een of andere class toepassen. '' is geen class.
    :disabled="JavaScript-voorwaarde" //werking uitzetten van wanneer waar
    <!-- voorbeeld: 
        Let op: Bij gebruik CSS naam gebruik of: (Camel vs Kebab)
            backgroundColor
            'background-color'    
    -->
        <div 
            v-for="variant in variants" :key="variant.id"   @mouseover="updateImage(variant.image)" 
            class="color-circle" 
            :style="{ backgroundColor: variant.color }">
        </div>
        <button 
            class="button" 
            :class="{ disabledButton: !inStock }" 
            :disabled="!inStock" 
            @click="addToCart">
            Add to Cart
        </button>
        <div :class="[isActive ? activeClass : '']"></div>
----------------------
<!-- Components & Props -->
Submap: components --> CompX.js
    app.component('comp-x', {
        props: {  //variabele die als input kunnen dienen
            variabele2c: {
                type: Boolean
                required: true
            }
        }
        template: //Opmaak
        /*html*/ 
        `
            <div>
            .....
            </div>
        `,
        data() {
            variabele1c: 'waarde'
        },
        methods: {
            methode2c() {
                this.$emit('trigger2c', variabele1c)
            }
        },
        computed: {
            bereken() {
                return this.variabele + ' ' + this.variabele
            },  
        }
    })

In index.html:
<!-- plaats Components -->
<comp-x :variabele2c="variabele2g" @trigger2c="methode2g"></comp-x>
<!-- Import Components -->
<script src="./components/CompX.js"></script>

In main.js:
    const app = Vue.createApp({
        data() {
            return {
                variabele2g: true,
                cart: [],
                ...
            }
        },
        methods: {
            methode2g(variabele1g) { //variabele1g = variabele1c uit component
                this.cart.push(variabele1g)
            }
        }
    })
--------------
<!-- Input(Form)/Binding (waarde 2 richtingsverkeer (koppelen)) -->
    v-model="JavaScript-waarde"
<!-- voorbeeld: -->
    In ComponentInput.js
        app.component('review-form', {
        template:
            /*html*/
            `<!-- inputbox, verbinden met  variabele "name" data -->
            <form @submit.prevent="onSubmit">
                <label for=""nameid">naamlabel</label>
                <input id="nameid" v-model="name" />
                <label for="ratingid">Ratinglabel</label>
                <select id="ratingid" v-model.number="rating">
                    <option>3</option>
                    <option>2</option>
                    <option>1</option>
                </select>
                <input type="submit" value="knoptekst">
            </form>
            `,
            data() {
                return {
                    name: '',
                    rating: null
                }
            },
            methods: {
                onSubmit() {
                    let productReview = {
                        name: this.name,
                        rating: this.rating,
                    }
                    this.$emit('input-submitted', productReview)

                    this.name = ''
                    this.rating = null
                }
           }
    in index.html
        <!-- plaats input form component-->
        <review-form @input-submitted="addInput"></review-form>
        <!-- Import Components -->
        <script src="./components/ReviewForm.js"></script>
    in main.js
        data() {
            return {
                ...
                reviews: []
            }
        },
        methods: {
            ...
            addInput(review) { //review = productRevies
                this.reviews.push(review)
            }
        },