<template>
    <div class="container-shopping_art">

        <div class="container-shopping_art__header">
            <h2>My Shopping Cart</h2>
        </div>

        <div class="container-shopping_art__cart">

            <div class="basket">

                <div class="basket-header">
                    <div class="basket-header__inner">
                        <div class="description-header">Item Description</div>
                        <div class="quantity-header">Quantity</div>
                        <div class="price-header">Price</div>
                        <div class="total-header">Total</div>
                    </div>
                </div>

                <div class="basket-cart">

                    <div class="basket-cart__inner" v-for="(item, index) in shoppingCart"
                         :key="index">

                        <div class="basket-cart__description">
                            <div>
                                <img class="img-music" :src="item.preview" alt="img-music">
                            </div>

                            <div>
                                <h4 class="basket-cart__title">{{ item.title }}</h4>
                                <p class="basket-cart__artist">{{ item.artist }}</p>
                            </div>

                        </div>

                        <div class="basket-cart__quantity">

                            <p class="quantity-block">
                                <span class="border dec"
                                      @click="subtractDec(index)"> - </span>
                                <span class="border">{{ item.quantity }}</span>
                                <span class="border inc"
                                      @click="addInc(index)"> + </span>
                            </p>
                            <p class="remove" @click="remuve(index)">Remove item</p>
                        </div>

                        <div class="basket-cart__price">
                            <p class="price">${{ item.price }}</p>
                            <p class="print">(${{ item.additionalPrint }}/addl.print)</p>
                        </div>

                        <div class="basket-cart__total">
                            <p>{{ item.totalPrice.toFixed(2) }} {{ totalPrice(index)}}</p>

                        </div>
                    </div>
                </div>
            </div>

            <div class="order-summary" v-for="(elem, index) in userFromApp"
                 :key="index">

                <h4>MY ORDER SUMMARY</h4>
                <hr>

                <p class="class">
                    Subtotal:
                    <span class="class">$ {{ totalSum }}</span>
                </p>

                <p class="class">
                    Shipping & Handling:
                    <span class="class">$ {{ purcent(elem.shipping) }}</span>
                </p>

                <p class="class">
                    Discount:
                    <span class="class">$ {{ purcent(elem.discount) }}</span>
                </p>

                <p class="class">
                    Coupon:
                    <span class="class">$ {{ purcent(elem.coupon) }}</span>
                </p>

                <p class="class">
                    Tax:
                    <span class="class">$ {{ purcent(elem.tax) }}</span>
                </p>

                <p class="class">
                    Gift Card:
                    <span class="class">$ {{ elem.giftCard }}</span>
                </p>

                <h4 class="class">
                    ORDER TOTAL
                    <span class="class">$ {{ purcentArr(elem) }} (USD)</span>
                </h4>
                <div class="order-button">
                    <button class="complete-order"
                            @click="sendOrder()">COMPLETE ORDER
                    </button>
                </div>
            </div>
        </div>
    </div>

</template>

<script>
    export default {
        name: "ShoppingCart",
        props: {
            shoppingCart: {
                type: Array,
                default() {
                    return {}
                }
            },
            userFromApp: {
                type: Array,
                default() {
                    return {}
                }
            }
        },
        data() {
            return {
                subtotal: 0,
                orderTotal: [],
            }
        },
        computed: {
            totalSum() {
                let arrPrice = []
                this.shoppingCart.map((elem) => {
                    arrPrice.push(elem.totalPrice)
                    this.subtotal = arrPrice.reduce((total, amount) => total + amount).toFixed(2)
                });
                return this.subtotal
            },
        },
        methods: {
            purcentArr(userDiscaunt) {
                let sumPurcent = this.subtotal - (((userDiscaunt.shipping + userDiscaunt.discount + userDiscaunt.coupon + userDiscaunt.tax) * this.subtotal) + userDiscaunt.giftCard)
                this.orderTotal = sumPurcent.toFixed(2) > 0 ? sumPurcent.toFixed(2) : this.subtotal
                return sumPurcent.toFixed(2) > 0 ? sumPurcent.toFixed(2) : this.subtotal
            },
            purcent(value) {
                value = value * this.subtotal;
                return value.toFixed(2)
            },
            subtractDec(index) {
                this.$emit('my-subtractDec', index);
            },
            addInc(index) {
                this.$emit('my-addInc', index);
            },
            totalPrice(index) {
                this.$emit('my-totalPrice', index);
            },
            remuve(indexFromCart) {
                if (this.shoppingCart.length === 1) {
                    this.subtotal = 0
                }
                this.$emit('my-remuve', indexFromCart)
            },
            sendOrder() {
                let idOrderTotal = [];
                this.shoppingCart.map((el) => {
                    idOrderTotal.push(el.id)
                });

                let order = {
                    name: "John Smith",
                    idOrderTotal: idOrderTotal,
                    priceTotal: this.orderTotal,
                };

                let orderJSON = JSON.stringify(order);
                console.log(orderJSON);
            }
        }
    }
</script>

<style scoped>
    .container-shopping_art {
        margin: 0 auto;
        width: 1150px;
    }

    .container-shopping_art__header {
        width: 100%;
    }

    .container-shopping_art__cart {
        display: flex;
        width: 100%;
    }

    .basket-header__inner {
        display: flex;
        height: 30px;
        color: white;
        background-color: black;
    }

    .description-header {
        width: 380px;
        padding: 5px 0 0 15px;
        border: 1px solid white;
    }

    .quantity-header {
        width: 150px;
        padding: 5px 0 0 15px;
        border: 1px solid white;
    }

    .price-header {
        width: 150px;
        padding: 5px 0 0 15px;
        border: 1px solid white;
    }

    .total-header {
        width: 150px;
        padding: 5px 0 0 95px;
        border: 1px solid white;
    }

    .basket-cart__inner {
        display: flex;
    }

    .basket-cart__description {
        display: flex;
        width: 380px;
        padding: 0 0 0 15px;
    }

    .img-music {
        height: 200px;

        background-color: rgb(255, 255, 255);
        box-shadow: rgb(0 0 0 / 26%) 0 0 10px 0;
        box-sizing: border-box;
        color: rgb(2, 114, 179);
        margin-top: 10px;
        margin-bottom: 10px;
        padding: 10px;;
    }

    .basket-cart__title {
        padding: 0 0 0 15px;
        color: rgb(2, 114, 179);
        font-size: 17px;
        font-weight: 700;
    }

    .basket-cart__artist {
        padding: 0 0 0 15px;
        color: rgb(102, 102, 102);
        font-size: 12px;
        font-weight: 400;
    }

    .basket-cart__quantity {
        width: 150px;
        padding: 0 0 0 15px;
    }

    .quantity-block {
        display: grid;
        grid-template-columns: 30px 40px 30px;
    }

    .inc, .dec {
        cursor: pointer;
    }

    .remove {
        color: #0272b3;
        cursor: pointer;
    }

    .border {
        border: 1px solid rgb(204, 204, 204);
        text-align: center;
    }

    .basket-cart__price {
        width: 150px;
        font-weight: 700;
        padding: 0 0 0 15px;
    }

    .price {
        font-weight: 700;
    }

    .print {
        opacity: 0.7;
        font-weight: 400;
        font-size: .8rem;
    }

    .basket-cart__total {
        width: 150px;
        font-weight: 700;
        padding: 0 0 0 95px;
    }


    .order-summary {
        background-color: rgb(239, 239, 239);
        margin: 0 auto 0 10px;
        padding: 0 10px 0 10px;
        width: 100%;
        height: 385px;
    }

    .order-summary span {
        float: right;
    }

    .complete-order {
        background-color: rgb(162, 188, 53);
        border-radius: 50px;
        color: rgb(255, 255, 255);
        cursor: pointer;
        font-size: 15px;
        font-weight: 700;
        border: none;
        padding: 10px 35px 10px 35px;
        text-align: center;
        width: 100%;
        outline: none;
    }

</style>
