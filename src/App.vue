<template>
    <div id="app">
        <h1>Bitcoin Price Index</h1>

        <section v-if="errored">
            <p>Unable to access Coindesk API.</p>
        </section>
        
        <section v-else>
            <div v-if="loading">Loading...</div>
            
            <div v-else v-for="currency in info" class="currency">
                {{ currency.description }}
                <span class="lighten">
                    <span v-html="currency.symbol"></span>{{ currency.rate_float | currencydecimal }}
                </span>
            </div>
        </section>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    filters: {
        currencydecimal (value) {
            return value.toFixed(2);
        }
    },
    data () {
        return {
            info: null,
            loading: true,
            errored: false
        };
    },
    mounted () {
        axios
         .get('https://api.coindesk.com/v1/bpi/currentprice.json')
         .then(response => (this.info = response.data.bpi))
         .catch(error => {
            console.log(error);
            this.errored = true;
         })
         .finally(() => this.loading = false);
    }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
