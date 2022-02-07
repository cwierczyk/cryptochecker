<template>
    <div @click="() => $router.push(`/${item.id}`)" class="item">
        <span class="item__badge">{{ index + 1 }}</span>
        <div class="item__content flex">
            <img class="item__content--image" :src="item.imageUrl" :alt="`${item.name} logo`">
            <span class="item__content--name">{{ item.name }}</span>
            <span class="item__badge">{{ item.symbol.toUpperCase() }}</span>
        </div>
        <span>{{ item.price }} €</span>
        <span :class="`${item.priceChange > 0 ? 'item__text-green' : 'item__text-red'} mobile-hidden`">{{ item.priceChange }} %</span>
        <span class="mobile-hidden">{{ formatAmount(item.totalVolume) }} €</span>
    </div>
</template>

<script>
export default {
    props: {
        id: {
            type: String,
            required: true
        },
        index: {
            type: Number,
            required: true
        },
        image: {
            type: String,
            required: true
        },
        name: {
            type: String,
            required: true
        },
        symbol: {
            type: String,
            required: true
        },
        price: {
            type: Number,
            required: true
        },
        priceChange: {
            type: Number,
            required: true
        },
        totalVolume: {
            type: Number,
            required: true
        }
        
    },
    setup(props) {
        const item = {
            id: props.id,
            index: props.index,
            imageUrl: props.image,
            name: props.name,
            symbol: props.symbol,
            price: props.price,
            priceChange: props.priceChange,
            totalVolume: props.totalVolume
        }

        const formatAmount = (amount) => {
            let formatter = new Intl.NumberFormat('en-US', {
                style: 'currency',
                currency: 'EUR',
            });

            return formatter.format(amount).substr(1)
        }

        return {
            item,
            formatAmount
        }
    },

}
</script>

<style lang="scss" scoped>

.item {
    display: grid;
    grid-template-columns: 0.1fr 1fr 0.5fr;
    align-items: center;
    border-bottom: 1px solid rgba($color: #000000, $alpha: 0.25);
    padding: 0.5rem 1rem;
    cursor: pointer;

    @media (min-width: 768px) {
        grid-template-columns: 0.25fr 2fr repeat(3, 1fr);
    }

    &__badge {
        font-weight: 500;
        opacity: 0.35;
    }

    &__content {
        align-items: center;

        &--image {
            width: 32px;
            height: 32px;
        }

        &--name {
            margin: 0 1rem;
        }
        
    }

    &__text-green {
        color: #1D8957;
    }
    
    &__text-red {
        color: #DC3545;
    }
}
</style>