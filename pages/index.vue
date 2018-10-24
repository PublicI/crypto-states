<template>
    <statebin :rows="rows" />
</template>

<script>
import rows from '~/assets/State Ethics Offices - State List.csv'
import Statebin from '~/components/Statebin.vue';

export default {
    data() {
        return {
            rows: rows.map(row => {
                return {
                    state: row.State,
                    regulations: row.Regulations ? row.Regulations : 'No data'
                };
            })
        };
    },
    components: {
        Statebin
    }
};
</script>

<style scoped>

</style>
