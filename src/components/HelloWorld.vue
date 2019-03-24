<template>
    <div class="hello">
        <nav>Hello, {{ msg }}</nav>
        <section>
            <h3>last BIG transactions on watched wallets</h3>
            <ol>
                <li v-for="item in transactions">
                    ${{item.amount}} from {{item.address}}
                </li>
                <li>$25 from RY23432423423</li>
            </ol>
            <button @click="getUpdates">update data</button>
        </section>
        <section>

            <label for="">Address for watching
                <input v-model.trim="newaddr" placeholder="E2035323223523" type="text"></label>
            <br>
            <label>name: <input v-model.trim="newname" placeholder="Whale pink" type="text"></label>
            <button @click="addAddr" v-if="newaddr">Add</button>
        </section>
        <section>
            <ol>
                <li v-for="item in wallets" :key="item.address">
                    {{item.address}}
                    <button>Stop watching</button>
                </li>
            </ol>
        </section>

    </div>
</template>

<script>
    export default {
        name: 'HelloWorld',
        props: {
            msg: String
        },
        data: function () {
            return {
                clientid: 123,
                wallets: [{address: 'EA333242342'}, {address: "TE435435435"}],
                transactions: [{address:'RT5435435345',amount:'1000'},],
                newaddr: null,
                newname: null,
            }
        },
        methods: {
            addAddr: function () {
                this.wallets.push({address: this.newaddr, name: this.newname});
                this.newaddr = null;
                this.newname = null;
            },
            getUpdates: function () {

                fetch('https://api.kanye.rest/')

                    .then(response => response.json())
                    .then(json => {
                            let amount = Math.floor(Math.random() * (1000)) + 1;
                            if (amount > 800) {
                                console.log('new money on ' + json.id + ' amount:' + amount);
                                this.transactions.push({address:json.id,amount:''+amount});
                            }
                        }
                    );
            }
        },
        created: function () {

            setInterval(() => {
                this.getUpdates();
            }, 800)
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    h3 {
        margin: 15px 0 0;
    }

    ul {
        padding: 0;
    }

    li {
        display: block;
        margin: 0 10px;
    }

    a {
        color: #42b983;
    }
</style>
