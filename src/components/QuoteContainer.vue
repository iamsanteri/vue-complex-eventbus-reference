<template>
    <div>
        <div class="card-columns">
            <div class="card" v-for="(quote, index) in quotesArray" @click="deleteCard(quote, index)">
                <div class="card-body">
                    <quoteCardText>{{ quote }}</quoteCardText>
                </div>
            </div>
        </div>
        <div v-if="alertVisible" class="alert alert-warning alert-dismissible fade show text-center" role="alert">
            <strong>You are out of space for quotes!</strong> Try deleting a couple of them by clicking.
        </div>
    </div>
</template>
<script>
    import { EventBus } from "./../main.js"
    import quoteCardText from "./quoteCardText.vue"
    
    export default {
        data: function() {
            return {
                quotesArray: [],
                alertVisible: false,
                one: 1
            }
        },
        methods: {
            saveToArray(payload) {
                this.quotesArray = payload;
            },
            importAlert(payload) {
                this.alertVisible = payload;
            },
            deleteCard(quote, index) {
                if(this.quotesArray[index] === quote) { 
                // The template passes index as the second parameter to avoid indexOf, 
                // it will be better for the performance especially for one large array
                // (because indexOf actually loop the array to do the match)
                this.quotesArray.splice(index, 1)
                EventBus.$emit("minusOne", this.one)
                } else {
                    let found = this.quotesArray.indexOf(item)
                    this.quotesArray.splice(found, 1)
                }
            }
        },
        mounted() {
            EventBus.$on("quotesArrayNew", (payloadQuotes) => {
                this.saveToArray(payloadQuotes);
            });

            EventBus.$on("alertOn", (booleanPayload) => {
                this.importAlert(booleanPayload);
            });
        },
        components: {
            quoteCardText
        }
    }
</script>
<style>
    .card-columns {
        margin-top: 20px; 
    }
    .card:hover {
        background-color: lightcoral;
    }
</style>