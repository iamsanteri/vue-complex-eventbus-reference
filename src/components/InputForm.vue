<template>
    <div class="container">
        <div class="row">
            <div class="col"></div>
            <div class="col-8">
                <form>
                    <div class="form-group">
                        <label>Quote</label>
                        <textarea v-model="typedQuote" class="form-control" rows="3"></textarea>
                    </div>
                </form>
                <div class="text-center">
                        <button class="btn btn-primary" @click="sendQuote">Add Quote</button>
                </div>
            </div>
            <div class="col"></div>
        </div>
    </div>
</template>
<script>
    import { EventBus } from "../main.js"

    export default {
        data: function() {
            return {
                typedQuote: ""
            }
        },
        methods: {
            sendQuote() {
                EventBus.$emit("quoteSent", this.typedQuote);
                this.typedQuote = ""; 
            }
        }
    }
</script>
<style>
</style>