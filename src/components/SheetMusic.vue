<template>
    <div>
        <div class="logo">
            <img alt="Vue logo" src="../assets/logo.png">
        </div>
        <div class="musicContainer">

            <div v-for="(elem, index) in groupsArrFromSheetMusic"
                 :key="index">

                <div>
                    <p v-if="elem.id !==2">{{ elem.name}}</p>
                </div>

                <div class="sheet-music" v-if="elem.id !=2">

                    <div v-for="(sheet, index) in sheetsArrFromSheetMusic"
                         :key="index">

                        <div class="sheet-music_in" v-if="sheet.sheetGroups !=2">
                            <img class="sheet-music_img" :src="sheet.preview" alt="">

                            <div class="song-desc">
                                <p class="musicTitle">{{ sheet.title }}</p>
                                <p class="musicArtist">{{ sheet.artist }}</p>
                                <p class="musicCategories">{{ sheet.sheetCategories.join(", ") }}</p>
                                <button class="buttonCart"
                                        @click="addToCart(index, sheet)">
                                    Add to cart
                                </button>
                            </div>
                        </div>
                    </div>
                </div>

                <div>
                    <p v-if="elem.id !==1">{{ elem.name}}</p>
                </div>

                <div class="sheet-music" v-if="elem.id !=1">

                    <div v-for="(sheet, index) in sheetsArrFromSheetMusic"
                         :key="index">

                        <div class="sheet-music_in" v-if="sheet.sheetGroups !=1">
                            <img class="sheet-music_img" :src="sheet.preview" alt="">

                            <div class="song-desc">
                                <p class="musicTitle">{{ sheet.title }}</p>
                                <p class="musicArtist">{{ sheet.artist }}</p>
                                <p class="musicCategories">{{ sheet.sheetCategories.join(", ") }}</p>
                                <button class="buttonCart"
                                        @click="addToCart(index, sheet)">
                                    Add to cart
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

</template>

<script>
    export default {
        name: "SheetMusic",
        props: {
            groupsArrFromSheetMusic: {
                type: Array,
                default() {
                    return {}
                }
            },
            sheetsArrFromSheetMusic: {
                type: Array,
                default() {
                    return {}
                }
            }
        },
        data() {
            return {}
        },
        methods: {
            addToCart(indexIn, dataMusic) {
                this.$emit('my-super-input', {indexIn, dataMusic});
            }
        },
    }
</script>

<style scoped>
    .logo {
        text-align: center;
    }

    .musicContainer {
        margin: 0 auto;
        padding: .2rem 3rem 1rem 3rem;
        width: 70vw;
        background: #efefef;
    }

    .sheet-music {
        width: 100%;
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
    }

    .sheet-music_in {
        --width-sheet-music_in: 10rem;
        width: var(--width-sheet-music_in);
        height: 13.5rem;
        margin: 0 1rem 1rem 1rem;
        position: relative;
    }

    .musicTitle {
        color: #fff;
        font-size: 14px;
        padding-left: 10px;
        padding-top: 6px;
    }

    .musicArtist {
        padding-left: 10px;
        font-weight: 400;
        font-size: 14px;
        color: rgba(255, 255, 255, 0.5);
    }

    .musicCategories {
        position: absolute;
        width: 100px;
        padding-left: 10px;
        font-weight: 400;
        font-size: 14px;
        color: rgba(255, 255, 255, 0.5);
        bottom: 0;
    }

    .sheet-music_img {
        width: 100%;
        box-shadow: rgba(0, 0, 0, 0.26) 0 0 1rem 0;
    }

    .song-desc {
        --width-song-desc: 11.5rem;
        --width-calc: calc(var(--width-song-desc) - var(--width-sheet-music_in));
        width: var(--width-song-desc);
        height: 50%;
        background-color: #0272b3;
        border-radius: .19rem;
        box-shadow: 0 -.125rem .44rem rgba(0, 0, 0, .33);
        border-bottom: .31rem solid rgba(0, 0, 0, .15);
        position: relative;
        transform: translate(0, -100%);
        right: calc(var(--width-calc) / 2);
    }

    .buttonCart {
        display: inline-block;
        float: right;
        right: 8px;
        bottom: 4px;
        background-color: white;
        color: rgb(2, 114, 179);
        border-radius: 10px;
        position: absolute;
        cursor: pointer;
        border: none;
        padding: 5px 5px 5px 5px;
    }

    .buttonCart:focus {
        outline: none;
    }

    .buttonCart:hover {
        box-shadow: 0 5px 5px lightgrey;
    }
</style>