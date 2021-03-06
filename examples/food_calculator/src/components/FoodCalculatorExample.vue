<template>
    <div>
        <tree-table
                class="table"
                :columns="columns"
                :table-data="tableData"
        >
            <template #headerTemplate="headerProps">
                <food-calculator-table-header v-bind="headerProps" />
            </template>

            <template #nodeTemplate="nodeProps">
                <food-calculator-node v-bind="nodeProps"
                                      :is-key-editable="isKeyEditable(nodeProps.depth)"
                                      :delete-node="deleteNode"
                                      :add-node="addNode(nodeProps.depth)"
                                      :keys-to-calculate="keysToCalculate" :columns="columns"/>
            </template>

            <template #leafTemplate="leafProps">
                <food-calculator-leaf v-bind="leafProps"
                                      :delete-node="deleteNode"
                                      :is-key-editable="isKeyEditable(leafProps.depth)" :columns="columns"/>
            </template>
        </tree-table>
        <div class="add-button" @click="addNode(topLevel)(tableData)">Add new record</div>
    </div>
</template>

<script>
    import TreeTable from 'vue-tree-table-component'
    import data from '../resources/data'
    import FoodCalculatorNode from "./FoodCalculatorNode";
    import FoodCalculatorLeaf from "./FoodCalculatorLeaf";
    import addNode from "../utils/addNode";
    import deleteNodeRecursively from "../utils/deleteNodeRecursively";
    import computeInitialValues from "../utils/computeInitialValues";
    import FoodCalculatorTableHeader from './FoodCalculatorTableHeader'

    export default {
        name: 'FoodCalculatorExample',
        components: {FoodCalculatorTableHeader, TreeTable, FoodCalculatorNode, FoodCalculatorLeaf },
        props: {
        },
        data: function() {
            return {
                tableData: data.tableData,
                columns: data.columns,
                editableFields: {
                    0: ['day', 'day_name'],
                    1: ['meal'],
                    2: ['ingredient', 'carbs','proteins', 'fat', 'kcal']
                },
                keysToCalculate: ['carbs','proteins', 'fat', 'kcal']
            }
        },
        computed: {
          topLevel: function(){ return 0 }
        },
        methods: {
            addNode,
            deleteNode(uuid){
                deleteNodeRecursively(this.tableData, uuid)
            },
            isKeyEditable(depth){
                const editableFields = this.editableFields[depth] || []
                return (key) => editableFields.includes(key)
            },
        },
        created() {
            computeInitialValues(this.tableData, this.keysToCalculate)
        }
    }
</script>

<style scoped>
    .table{
        margin: auto;
    }

    .add-button{
        width: 95%;
        margin: auto;
        text-align: right;
        color: green;
        padding: 1rem;
        padding-right: 2.5rem;
        font-weight: bold;
    }
</style>
