<template>
    <div id='defaultForm'>
    </div>
</template>

<script>
    /*
        Simple way to use InnerSearch
    */

    import Vue from 'vue';
    import InnerSearch from "../innerSearch.js";
    import '../src/style.css';

    Vue.use(InnerSearch);

    window.addEventListener('load', function () {
        new Vue({
            el: '#defaultForm',

            created : function () {
                // ES server configuration
                this.setHost('http://77.158.35.141:9200');
                this.setIndex('opf');
                this.setType('programs');
            },

            template : `
                <section>

                <h1 class='is-title'>innerSearch.js</h1>

                <hr class='is-line' />

                <div>
                    <searchbox :autofocus="true" :realtime="true" :field="'DESCRIPTION'" :pattern="'.*{v}.*'" :placeholder="'Search by keywords'" :suggestionbox="true">
                        <template slot="suggestions" slot-scope="{ suggestion }">
                            <div>{{ suggestion.DESCRIPTION.substr(0, 100) }}...</div>
                        </template>
                    </searchbox>
                    <refinement-list-filter :field="'SITE'" :size="10" :title="'Website : '" :dynamic="true" orderKey="_count" orderDirection="desc" operator="AND"></refinement-list-filter>
                    <refinement-list-filter :field="'CODE_POSTAL'" :size="10" :title="'Code postal : '" :dynamic="false" orderKey="_count" orderDirection="desc" operator="AND"></refinement-list-filter>
                </div>

                <hits>
                    <template slot="hits" slot-scope="{ hits }">
                        <div class="is-score is-hits">
                            <strong v-if="hits.score === 0">No result found</strong>
                            <strong v-else-if="hits.score === 1">1 result found</strong>
                            <strong v-else-if="hits.score > 1">{{ hits.score }} results found</strong>
                        </div>
                        <div v-for="item in hits.items" :item="item">
                            <div style="margin : 10px;">
                                <p><strong>NOM :</strong> {{ item._source.NOM }}</p>
                                <p><strong>URL :</strong> {{ item._source.URL }}</p>
                                <p><strong>PROMOTEUR :</strong> {{ item._source.PROMOTEUR }}</p>
                                <p>{{ item._source.DESCRIPTION }}</p>
                            </div>
                        </div>
                    </template>
                </hits>

                </section>
            `
        });
    });

    export default {};
</script>
