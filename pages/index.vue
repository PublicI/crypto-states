<template>
    <div>
        <h4>{{allowed.length-1}} and D.C. states allow cryptocurrency contributions</h4>
        <statebin :rows="allowed" :labels="['Allowed']" :colors="['#73AF48']" />

        <h4>{{banned.length}} states ban cryptocurrency contributions</h4>
        <statebin :rows="banned" :labels="['Banned','Banned in House']" :colors="['#CC503E','#E17C05']" />

        <h4>{{none.length}} states are debating or have no policy on cryptocurrency contributions</h4>
        <statebin :rows="none" :labels="['Debating','None']" :colors="['#994E95','#666666']" />
    </div>
</template>

<script>
import rows from '~/assets/State Ethics Offices - State List.csv'
import Statebin from '~/components/Statebin.vue';

export default {
    data() {
        let states = rows.map(row => {
            return {
                state: row.State,
                regulations: row.Regulations ? row.Regulations : 'No data'
            };
        });

        return {
            allowed: states.filter(row => row.regulations === 'Allowed'),
            banned: states.filter(row => row.regulations === 'Banned' || row.regulations === 'Banned in House'),
            none: states.filter(row => row.regulations === 'None' || row.regulations === 'Debating')
        };
    },
    components: {
        Statebin
    }
};
</script>

<style scoped>
h4 {
    max-width: 280px;
    line-height: 115%;
}
</style>
