<template>
    <div class="main-container d-flex">
        <div class="input-table-container">
            <input-add @addItem="addItemWithChecks" :errMsgs="errorMessage"/>
            <comparement-table :scores-values="itemsValues" :key="componentsKey"/>
        </div>
        <div class="radio-container">
            <h1 class="radio-header">Compare items</h1>
            <v-radio-group 
                v-for="(pairs, i) in comparisonPairs" 
                v-model="checkedValues[i]" 
                :key="pairs.join()" 
                row 
                class="pairs-row d-flex"
                @change="onChangeHandler()"
            >
                <v-radio
                    v-for="item in pairs"
                    :id="item+i"
                    :key="item+i"
                    class="item-container"
                    :label="item"
                    :value="item"
                ></v-radio>
            </v-radio-group>
        </div>
    </div>
</template>

<script>
import ComparementTable from "./ComparementTable.vue";
import InputAdd from "./InputAdd.vue";

export default {
    name: "DoubleComparison",
    components: { ComparementTable, InputAdd },
    data: () => ({
        itemsValues: {},
        itemsArray: [],
        comparisonPairs: [],
        checkedValues: [],
        componentsKey: 0,
        searchText: "",
        errorMessage: []
    }),
    created() {
        for (const item of ["A", "B", "C"]) {
            this.addItem(item);
        }
    },
    methods: {
        forceRender() {
            this.componentsKey ++;
            // We need a forceRender, because of use a "itemsValues" in both commponents
        },
        createPairs(arr1, arr2) {
            for (let i = 0; i < arr1.length; i++) {
                for (let j = 0; j < arr2.length; j++) {
                    this.comparisonPairs.push([arr1[i], arr2[j]]);
                }
            }
        },
        addItem(value) {
            this.itemsValues[value] = 0;
            this.createPairs(this.itemsArray, value);
            this.itemsArray.push(value);
        },
        addItemWithChecks(inputVal) {
            if (this.itemsArray[0].indexOf(inputVal.toUpperCase()) === -1) {
                if (inputVal.length <= 1) {
                    if (inputVal.length) {
                        this.addItem(inputVal);
                        this.errorMessage = [];
                        this.forceRender();
                    } else {
                        this.errorMessage.push("Input shouldn't be empty");
                    }
                } else {
                    this.errorMessage.push("Max length exceeded");
                }
            } else {
                this.errorMessage.push("The item is already exists");
            }
        },
        onChangeHandler() {
            for (const property in this.itemsValues) {
                this.itemsValues[property] = this.checkedValues.filter(item => item === property).length;
                this.forceRender();
            }
        },
    }
}
</script>

<style lang="scss" scoped>
.main-container {
    justify-content: space-around;
    margin: 100px 0;
    .input-table-container {
        width: 300px;
    }
    .radio {
        &-container {
            &-header {
                font-weight: 600;
                margin: 0 10px;
            }
        }
    }
    .pairs-row{
        .item{
            &-container {
                position: relative;
                width: 30px;
                height: 30px;
                margin: 10px;
                border-radius: 3px;
                border: 1px solid #f2f2f2;
                color: #bebebe;
                &:active {
                    transform: scale(0.9)
                }
            }
        }
    }
}
</style>