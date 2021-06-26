<template>
    <div class="input-container">
        <v-text-field 
            outlined 
            v-model="inputText" 
            placeholder="New Item" 
            :hide-details="!errMsgs.length" 
            :error-messages="errMsgs[errMsgs.length - 1]"
            @keyup.enter="passQuery"
        >
        </v-text-field>
        <v-btn 
            class="input-btn" 
            color="#80e8d9"
            absolute 
            @click="passQuery"
            large
        >Add</v-btn>
    </div>
</template>

<script>
export default {
    name: "InputAdd",
    props: {
        errMsgs: {
            type: Array,
            required: true
        }
    },
    data: () => ({
        inputText: ""
    }),
    methods: {
        passQuery() {
            this.$emit("addItem", this.inputText.toUpperCase());
            this.inputText = "";
        }
    }
}
</script>

<style lang="scss" scoped>
.input {
    &-container {
        position: relative;
        height: 56px
    }
    &-btn {
        &.v-btn {
            top: 0;
            right: 0;
            height: inherit;
            color: #FFFFFF;
            text-transform: capitalize;
            transform: translateX(100%);
            border-top-left-radius: 0;
            border-bottom-left-radius: 0;
            box-shadow: none;
        }
    }
}
</style>