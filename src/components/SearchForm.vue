<template>
    <v-main>
        <v-container style="padding: 5%">
            <v-form
                ref="form"
                lazy-validation
            >
                <v-text-field
                    v-model="address"
                    label="Address"
                    style="cursor: text;"
                    :rules="inputAddress"
                ></v-text-field>
                <v-text-field
                    v-model="token"
                    label="Token"
                    style="cursor: text;"
                    :rules="inputToken"
                ></v-text-field>
                <v-btn @click="submit" style="width: 100%;" outlined>
                    Submit
                </v-btn>
                <v-btn @click="clear" style="width: 100%; margin: 3% auto 0.5%;" outlined>
                    Clear
                </v-btn>
            </v-form>
            <v-container v-if="result">
                <ul v-if="result.suggestions.length != 0">
                    <onePlace v-for="(place, index) in result.suggestions" :key="index" :obj="place" />
                </ul>
                <v-container v-else style="text-align: center;">
                    Not a single place was found for your request.
                </v-container>
            </v-container>
            <v-container v-if="api_error" style="text-align: center;">
                You typed incorrect data. Please, check this.
            </v-container>
            <v-container v-if="error" style="text-align: center;">
                Server connection error. Please, repeat later.
            </v-container>
        </v-container>
    </v-main>
</template>

<script>
import onePlace from '@/components/onePlace.vue'

export default {
    components: {
        onePlace
    },
    data: () => ({
        address: "",
        token: "",
        inputAddress: [
            input => {
                if (input)
                    return true
                else 
                    return 'Type address in this field'
            }
        ],
        inputToken: [
            input => {
                if (input)
                    return true
                else
                    return 'Type token in this field'
            }
        ],
        result: "",
        api_error: false,
        error: "",
        key: 0,
    }),
    methods: {
        submit() {
            if (this.address && this.token) {
                console.clear()
                this.result = ""
                this.error = ""
                this.api_error = false
    
                const url = "https://suggestions.dadata.ru/suggestions/api/4_1/rs/suggest/address"
                const token = this.token
                const query = this.address
    
                var options = {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json",
                        "Accept": "application/json",
                        "Authorization": "Token " + token
                    },
                    body: JSON.stringify({query: query})
                }
    
                fetch(url, options)
                .then(response => {
                    if (response.status !== 200)
                        this.api_error = true
                    else
                        response.json().then(result => this.result = result )
                })
                .catch(error => this.error = error);
            }
        },
        clear() {
            this.$refs.form.reset()
            this.result = ""
            this.error = ""
            this.api_error = false
        }
    }
}

</script>

<style scoped>

.v-main {
    display: block; 
    width: 40%;
    margin: 3% auto 3% auto;
    border: 1px solid black;
    border-radius: 25px;
    background-color: rgb(255, 255, 255, 0.85);
    height: max-content;
    flex: 0;
}

</style>