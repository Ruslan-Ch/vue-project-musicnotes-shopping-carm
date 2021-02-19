<template>
    <div id="app">

        <div class="error-invisible" v-for="(error, i) in errors"
             :key="i"
             :class="{ errorOrange: isActive }">
            <h1 class="error-title">Ошибка GET запроса / {{ error }}</h1>
        </div>
        
        <SheetMusic :groupsArrFromSheetMusic="sheetGroupsArr"
                    :sheetsArrFromSheetMusic="sheetsArr"
                    @my-super-input="valueButton">
        </SheetMusic>

        <ShoppingCart :shoppingCart="shoppingCart"
                      :userFromApp="user"
                      @my-subtractDec="subtractDecQuantity"
                      @my-addInc="addIncQuantity"
                      @my-totalPrice="addTotalPrice"
                      @my-remuve="myRemuve">
        </ShoppingCart>

    </div>

</template>

<script>
    import SheetMusic from './components/SheetMusic.vue';
    import ShoppingCart from './components/ShoppingCart.vue';

    export default {
        name: 'App',
        components: {
            SheetMusic,
            ShoppingCart,
        },
        data() {
            return {
               urlSheets: 'https://raw.githubusercontent.com/brightestsirius/musicnotes/master/sheets.json',
                user: [
                    {
                        shipping: 0,
                        discount: .02,
                        coupon: .05,
                        tax: 0,
                        giftCard: .5
                    }
                ],
                errors: [],
                sheetCategoriesArr: [],
                sheetGroupsArr: [],
                sheetsArr: [],
                shoppingCart: [],
                isActive: false
            }
        },
        async mounted() {
            try {
                const response = await fetch(this.urlSheets);
                const result = await response.json();
                this.sheetCategoriesArr = result.sheetCategories;
                this.sheetGroupsArr = result.sheetGroups;
                this.sheetsArr = result.sheets;

                this.sheetsArr.map((item) => {
                    this.$set(item, 'quantity', 1)
                    this.$set(item, 'totalPrice', 0)

                    item.sheetCategories.map((itemSheetCategories, indexSheetCategories, arraySheetCategories) => {
                        this.sheetCategoriesArr.map((itemCategoriesArr, indexCategoriesArr, arrayCategoriesArr) => {

                            if (arraySheetCategories[indexSheetCategories] === arrayCategoriesArr[0].id) {
                                arraySheetCategories[indexSheetCategories] = arrayCategoriesArr[0].name
                            } else if (arraySheetCategories[indexSheetCategories] === arrayCategoriesArr[1].id) {
                                arraySheetCategories[indexSheetCategories] = arrayCategoriesArr[1].name
                            }
                        })
                    })
                })
            } catch (error) {
                this.errors.push(error)
                setInterval(() => this.isActive=true, 700);
                setInterval(() => this.isActive=false, 1400);
            }
        },

        methods: {
            valueButton(dataSheet) {
                if (this.shoppingCart.length) {
                    let isProductExist = false;
                    this.shoppingCart.map((item) => {
                        if (item.id === dataSheet.dataMusic.id) {
                            isProductExist = true;
                            item.quantity++
                        }
                    });

                    if (!isProductExist) {
                        this.shoppingCart.push(dataSheet.dataMusic)
                    }
                } else {
                    this.shoppingCart.push(dataSheet.dataMusic)
                }
            },
            subtractDecQuantity(index) {
                if (this.shoppingCart[index].quantity > 1) {
                    this.shoppingCart[index].quantity--
                }
            },
            addIncQuantity(index) {
                this.shoppingCart[index].quantity++
            },
            addTotalPrice(index) {
                let a = this.shoppingCart[index].quantity,
                    b = this.shoppingCart[index].price,
                    c = this.shoppingCart[index].additionalPrint;
                this.shoppingCart[index].totalPrice = a === 1 ? a * b : (a * b) - (a * c);
            },
            myRemuve(indexFromCart) {
                this.shoppingCart[indexFromCart].quantity = 1
                this.shoppingCart.splice(indexFromCart, 1)
            },
        }
    }

</script>

<style>
    * {
        box-sizing: border-box;
    }

    #app {

        font-family: "ProximaNova", "Helvetica Neue", Helvetica, Arial, sans-serif;
        /*-webkit-font-smoothing: antialiased;*/
        /*-moz-osx-font-smoothing: grayscale;*/
        /*text-align: center;*/
        /*color: #2c3e50;*/
        /*margin-top: 60px;*/
    }

    .error-invisible {
        background: red;
        border: 20px solid #2520ff;
        width: 43rem;
        height: 10rem;
        margin: 0 auto;
    }
    .errorOrange {
        background: #2520ff;
        border: 20px solid red;
    }

    .error-title {
        text-align: center;
    }
</style>

