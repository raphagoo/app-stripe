<template>
    <v-content class="home">
        <h1>Basket</h1>
        <v-card class="pa-3 ma-1 basket-items" v-for="item in basket" :key="item.id">
            <v-list-item three-line>
                <v-list-item-content>
                    <div class="text-overline mb-4">
                        Clothing
                    </div>
                    <v-list-item-title class="text-h5 mb-1">
                        {{item.name}}
                    </v-list-item-title>
                    <v-list-item-subtitle>{{item.price}}</v-list-item-subtitle>
                </v-list-item-content>

                <v-list-item-avatar
                    tile
                    size="80"
                    color="grey"
                ><img :src="item.picture" /></v-list-item-avatar>
            </v-list-item>
        </v-card>
        <stripe-checkout
            ref="checkoutRef"
            mode="payment"
            :pk="publishableKey"
            :line-items="lineItems"
            success-url="http://localhost:8080?success=1"
            cancel-url="http://localhost:8080?success=0"
            @loading="v => loading = v"
        />
        <v-btn class="yellow" elevation="2" @click="submit">Pay now!</v-btn>
    </v-content>
</template>

<script>
import { StripeCheckout } from '@vue-stripe/vue-stripe';
import Swal from "sweetalert2";

export default {
    name: 'home',
    mounted(){
        const urlParams = new URLSearchParams(window.location.search);
        const isSuccess = urlParams.get('success');
        if(isSuccess === '1'){
            Swal.fire({
                icon: 'success',
                title: 'Success',
                text: 'Payment is successful'
            })
        } else if(isSuccess === '0'){
            Swal.fire({
                icon: 'error',
                title: 'Error',
                text: 'Payment has been canceled'
            })
        }
    },
    components: {
        StripeCheckout
    },
    data() {
        return {
            publishableKey: "pk_test_51KFIasJrsFsbQyAc0YbKcmgH7Hhs67Kb70ZX6kYujkHqkMTFXJJGwXiMjCwUnTMsaZXa6r5RVlUTWhB3DOPmGokb00byRsR762",
            loading: false,

            lineItems: [
                {
                    quantity: 1,
                    price: "price_1KFIeZJrsFsbQyActGL9dnGq"
                },
                {
                    quantity: 1,
                    price: "price_1KFIfAJrsFsbQyAcQAR6izE5"
                }
            ],
            basket: [
                {
                    name: 'Tee-shirt noir',
                    picture: './assets/t-shirt.jpg',
                    quantity: 1,
                    price: "19 €"
                },
                {
                    name: 'Jean gris',
                    picture: './assets/jean.jpg',
                    quantity: 1,
                    price: "50 €"
                }
            ]
        }
    },
    methods: {
        submit () {
            // You will be redirected to Stripe's secure checkout page
            this.$refs.checkoutRef.redirectToCheckout();
        },
    },
}
</script>
<style lang="scss">
.basket-items{
    width: 50%;
}
</style>
