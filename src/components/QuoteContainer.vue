<template>
    <div>
        {{ quotesArray }}
    </div>
</template>
<script>
    import { EventBus } from "./../main.js"
    
    export default {
        data: function() {
            return {
                quotesArray: []
            }
        },
        methods: {
            saveToArray(payload) {
                this.quotesArray = payload;
            }
        },
        mounted() {
            EventBus.$on("quotesArrayNew", (payloadQuotes) => {
                this.saveToArray(payloadQuotes);
            });
        }
    }
</script>
<style>
</style>