<template>
    <div>
        <p class="chatter">The Center for Public Integrity reached out to all 50 states to ask if candidates running for state office may accept cryptocurrency. In total, {{responded.length-1}} states and D.C. responded.</p>

        <h4>At least {{apnumber(allowed.length-1)}} states and D.C. allow cryptocurrency contributions</h4>
        <statebin :rows="allowed" :labels="['Allowed']" :colors="['#73AF48']" />

        <h4>At least {{apnumber(banned.length)}} states ban cryptocurrency contributions</h4>
        <statebin :rows="banned" :labels="['Banned','Banned in House']" :colors="['#CC503E','#E17C05']" />

        <h4>At least {{apnumber(none.length)}} states are debating or have no policy on cryptocurrency contributions</h4>
        <statebin :rows="none" :labels="['Debating','None']" :colors="['#994E95','#666666']" />
    </div>
</template>

<script>
import rows from '~/assets/State Ethics Offices - State List.csv'
import Statebin from '~/components/Statebin.vue';
import { apnumber } from 'journalize';
import { csvParse } from 'd3';

export default {
    methods: {
        apnumber,
        capfirst(s) {
            s = s + '';
            return s.slice(0,1).toUpperCase() + s.slice(1);
        }
    },
    async asyncData({ app, error }) {
        const spreadsheetUrl =
            'https://docs.google.com/spreadsheets/d/e/2PACX-1vQpkpFPfD56KzMNefQ6vPBenErBa3slTawfrq_nFpE3ARRzstVQrQzS7Ii6OIdMvwxLln9V_gqATZKB/pub?gid=545548731&single=true&output=csv';
        let csv = await app.$axios.$get(spreadsheetUrl);
        let rows = await csvParse(csv);

        let states = rows.map(row => {
            return {
                state: row.State,
                regulations: row.Regulations
            };
        });

        return {
            responded: states.filter(row => row.regulations !== ''),
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
.chatter {
    max-width: 280px;
    font-size: 15px;
    line-height: 130%;
    color: rgb(100,100,100);
}
</style>
