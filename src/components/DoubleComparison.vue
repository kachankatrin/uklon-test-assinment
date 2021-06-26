<template>
    <div class="main-container d-flex">
        <div class="input-table-container">
            <input-add @addItem="addAnItem" :errMsgs="errorMessage"/>
            <comparement-table :scores-values="itemsValues" :key="componentsKey"/>
        </div>
        <div class="radio-container">
            <h1 class="radio-header">Compare items</h1>
            <v-radio-group 
                v-for="(pairs, i) in comparisonPairs" 
                v-model="isChecked[i]" 
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
        itemsValues: { A: 0, B: 0, C: 0 },
        itemsMatrix: [],
        comparisonPairs: [],
        isChecked: [],
        componentsKey: 0,
        searchText: "",
        errorMessage: []
    }),
    created() {
        this.convertToMatrix();
        this.createInitialComparisonPairs();
    },
    methods: {
        forceRender() {
            this.componentsKey ++;
        },
        convertToMatrix() {
            this.itemsMatrix.push(Object.keys(this.itemsValues));
        },
        createInitialComparisonPairs() {
            for (let i = 0; i < this.itemsMatrix[0].length - 1; i++) {
                for (let j = i + 1; j < this.itemsMatrix[0].length; j++) {
                    this.comparisonPairs.push([this.itemsMatrix[0][i], this.itemsMatrix[0][j]]);
                }
            }
        },
        createComparisonPairs() {
            for (let i = 0; i < this.itemsMatrix[0].length; i++) {
                for (let j = 0; j < this.itemsMatrix[1].length; j++) {
                    this.comparisonPairs.push([this.itemsMatrix[0][i], this.itemsMatrix[1][j]]);
                }
            }
        },
        addAnItem(inputVal) {
            if (this.itemsMatrix[0].indexOf(inputVal.toUpperCase()) === -1) {
                if (inputVal.length <= 1) {
                    if (inputVal.length) {
                    this.itemsMatrix.push([inputVal]);
                    this.itemsValues[inputVal] = 0;
                    this.createComparisonPairs();
                    this.itemsMatrix[0].push(inputVal);
                    this.itemsMatrix.pop();
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
                this.itemsValues[property] = this.isChecked.filter(item => item === property).length;
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