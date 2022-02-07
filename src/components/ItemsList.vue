<template>
    <div class="wrapper flex flex-col">
        <div class="wrapper__input flex">
            <input v-model="enteredName" placeholder="Search Coin" type="text">
        </div>
        <div class="wrapper__items flex flex-col">
            <header>
                <span>#</span>
                <span>Coin</span>
                <span>Price</span>
                <span class="mobile-hidden">Price Change</span>
                <span class="mobile-hidden">24 Volume</span>
            </header>
            <div class="wrapper__items--list flex flex-col">
                <list-item 
                v-for="(item, i) in paginatedItems" 
                :key="item.index" 
                :index="i"
                :id="item.id" 
                :image="item.image" 
                :name="item.name" 
                :symbol="item.symbol"
                :price="item.current_price" 
                :priceChange="item.price_change_percentage_24h"
                :totalVolume="item.total_volume" />
            </div>
        </div>
        <div class="wrapper__pagination flex">
            <button :disabled="currentPage - 1 < 1" @click="() => currentPage--">Previous</button>
            <button :class="currentPage === i + 1 ? 'active' : ''" :disabled="currentPage === i + 1" @click="() => currentPage = i + 1" v-for="(page, i) in pages" :key="i">{{ i + 1 }}</button>
            <button :disabled="currentPage + 1 > pages" @click="() => currentPage++">Next</button>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import ListItem from './ListItem.vue'
import { ref, computed, onMounted } from 'vue'


export default {
    components: { ListItem },
    props: {
        perPage: {
            type: Number,
            required: false,
            default: 14
        }
    },
    setup(props) {
        let items = ref([])
        let enteredName = ref('')
        let itemsPerPage = ref(props.perPage)
        let currentPage = ref(1)

        const getAllCryptocurrencies = async () => {
            const response = await axios.get('https://api.coingecko.com/api/v3/coins/markets?vs_currency=eur&order=market_cap_desc&per_page=100&page=1&sparkline=false')
            response.data.forEach((item, i) => {
                item.index = i
                items.value.push(item)
            })
        }

        const pages = computed(() => filteredItems.value.length <= 0 ? 1 : Math.ceil(filteredItems.value.length / itemsPerPage.value))

        const filteredItems = computed(() => {
            const inputValue = enteredName.value.toLowerCase()
            
            if (!inputValue) {
                return items.value
            } else {
                return items.value.filter((item) => {
                    const itemName = item.name.toLowerCase()
                    const itemBadge = item.symbol.toLowerCase()

                    return itemName.indexOf(inputValue) != -1 || itemBadge.indexOf(inputValue) != -1

                })
            }

        })

        const paginatedItems = computed(() => {
            return filteredItems.value.filter((item) => {
                if (pages.value === 1) {
                    return item
                } else {
                    return item.index >= (currentPage.value - 1) * itemsPerPage.value && item.index <= currentPage.value * itemsPerPage.value - 1
                }
                
            })
        })

        onMounted(() => {
            getAllCryptocurrencies()
        })

        return {
            enteredName,
            filteredItems,
            pages,
            currentPage,
            paginatedItems
        }
    },
}
</script>

<style lang="scss" scoped>

.wrapper {
    width: 100%;

    &__input {
        margin: 1.5rem 0;
        input {
            width: 100%;
            outline: none;
            border: none;
            padding: 0.5rem 1rem;
            border-radius: 4px;
            font-size: 1rem;
            transition: .25s ease all;

            &:focus {
                box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
            }
        }
    }

    &__items {
        header {
            display: grid;
            grid-template-columns: 0.1fr 1fr 0.5fr;
            border-bottom: 2px solid #000;
            padding: 0.5rem 1rem;

            @media (min-width: 768px) {
                grid-template-columns: 0.25fr 2fr repeat(3, 1fr);
            }

            span {
                font-weight: bold;
            }
        }
    }

    &__pagination {
        padding: 1rem 0;
        align-items: center;
        justify-content: center;
        flex-wrap: wrap;
        
        button {
            border-radius: 4px;
            background: rgba($color: #fff, $alpha: .5);
            border: 1px solid rgba($color: #000000, $alpha: .2);
            color: rgba($color: #000000, $alpha: .65);
            outline: none;
            padding: 0.5rem 1rem;
            margin: 0.25rem;
            cursor: pointer; 
            font-weight: bold;

            &.active {
                border: 2px solid rgba($color: #000000, $alpha: .5);
            }

            &:disabled {
                background: rgba($color: #fff, $alpha: .35);
                cursor: not-allowed;
                color: rgba($color: #000000, $alpha: .2);
            }

            
        }

    }
}
</style>