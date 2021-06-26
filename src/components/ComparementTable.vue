<template>
    <v-simple-table
        fixed-header
        height="300px"
        class="compare-table"
    >
        <template v-slot:default>
        <thead>
            <tr>
            <th class="text-left">
                Item
            </th>
            <th class="text-left">
                Score
            </th>
            </tr>
        </thead>
        <tbody>
            <tr
            v-for="(score, item) in sortedObj"
            :key="item"
            >
            <td>{{ item }}</td>
            <td class="d-flex align-center">{{ score }}
                <v-chip
                    v-if="score && score >= getTheLargestScore"
                    class="ma-2"
                    color="green"
                    text-color="white"
                    label
                >
                    Winner
                </v-chip>
            </td>
            </tr>
        </tbody>
        </template>
    </v-simple-table>
</template>

<script>
export default {
    name: "ComparementTable",
    props: {
        scoresValues: {
            type: Object,
            required: true
        }
    },
    data () {
      return {
        scores: []
      }
    },
    computed: {
        getTheLargestScore() {
            return Math.max.apply(null, Object.values(this.scoresValues));
        },
        sortedObj() {
            return Object
                .fromEntries(Object.entries(this.scoresValues).sort(([,a],[,b]) => b - a));
        }
    },
    methods: {
    }
}
</script>

<style lang="scss" scoped>
.compare-table {
    &.v-data-table {
        margin-top: 30px;
        .text-left, .text-right {
            color: #000000;
            font-size: 16px;
        }
        tr {
            td {
                &:first-of-type {
                    color: #000000;
                    font-weight: 600;
                }
                &:last-of-type {
                    justify-content: space-between;
                }
            }
        }
    }
}
</style>