<template>
    <section>
        <div class="container flex flex-col">
            <div class="wrapper flex flex-col">
                <header class="flex">
                    <img :src="details.logo" :alt="`${details.name} logo`" />
                    <div class="wrapper__details flex flex-col">
                        <h4>
                            {{ details.name }}
                            <span class="wrapper__details--symbol">{{ details.symbol.toUpperCase() }}</span>
                        </h4>
                        <span class="wrapper__details--year">Created: {{ details.creationYear }}</span>
                        <span>
                            Website:
                            <a :href="details.website" target="_blank" rel="noopener noreferrer">{{ details.website }}</a> 
                        </span>
                    </div>
                </header>
                <p class="wrapper__content" v-html="details.description"></p>
            </div>
            <a class="btn" href="/">Previous page</a>
        </div>
    </section>
</template>

<script>
import axios from 'axios'
import { ref, onMounted } from 'vue'
import { useRoute } from 'vue-router'

export default {
    setup() {
        const route = useRoute()

        let details = ref({
            name: '',
            symbol: '',
            logo: '',
            website: '',
            creationYear: '',
            description: ''
        })

        const setCurrencyDetails = async () => {
            const response = await axios.get(`https://api.coingecko.com/api/v3/coins/${route.params.id}`)

            details.value = {
                name: response.data.name,
                symbol: response.data.symbol,
                logo: response.data.image.large,
                website: response.data.links.homepage[0],
                creationYear: response.data.genesis_date,
                description: response.data.description.en
            }
        }

        onMounted(() => {
            setCurrencyDetails()
        })

        return {
            details,
            setCurrencyDetails
        }
    },
}
</script>


<style lang="scss" scoped>

.wrapper {
    background: #fff;
    box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
    padding: 2rem;
    border-radius: 4px;

    header {
        img {
            max-width: 128px;
        }
    }

    &__details {
        margin: 1rem 1.5rem;

        h4 {
            font-size: 2rem;
        }

        &--symbol {
            color: rgba($color: #000000, $alpha: .25);
        }

        &--year {
            margin: 0.5rem 0;
        }
    }

    &__content {
        padding: 2.5rem;
        line-height: 2rem;
        font-size: 1.15rem;
    }
}

.btn {
    margin: 1rem auto;
    padding: 0.5rem 1rem;
    border-radius: 4px;
    border: 2px solid rgba($color: #000000, $alpha: .35);
    background: rgba($color: #fff, $alpha: .35);
    text-decoration: none;
    font-weight: 600;
    color: rgba($color: #000000, $alpha: .5);
    transition: .35s ease all;

    &:hover {
        box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
        color: rgba($color: #000000, $alpha: .75);
        border: 2px solid rgba($color: #000000, $alpha: .75);
    }
}
</style>