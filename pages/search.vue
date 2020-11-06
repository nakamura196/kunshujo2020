<template>
  <client-only>
    <ais-instant-search
      :routing="routing"
      :search-client="searchClient"
      index-name="kunshujo"
    >
      <v-container class="my-5">
        <v-row>
          <v-col col="12" sm="4">
            <v-card flat outlined>
              <v-card-title>Tags</v-card-title>
              <v-card-text>
                <ais-refinement-list
                  show-more
                  operator="and"
                  :show-more-limit="100"
                  :limit="20"
                  attribute="tags"
                />
              </v-card-text>
            </v-card>
          </v-col>
          <v-col col="12" sm="8">
            <ais-search-box placeholder="Search here…" />

            <client-only>
              <ais-powered-by class="my-2" />
            </client-only>

            <v-sheet class="pa-4 my-4" color="grey lighten-3">
              <ais-stats>
                <p
                  class="my-0"
                  slot-scope="{
                    hitsPerPage,
                    nbPages,
                    nbHits,
                    page,
                    processingTimeMS,
                    query,
                  }"
                >
                  Page {{ page + 1 }} of {{ nbPages }} with
                  {{ hitsPerPage }} hits per page -
                  {{ nbHits.toLocaleString() }} hits retrieved in
                  {{ processingTimeMS }}ms
                </p>
              </ais-stats>
            </v-sheet>

            <ais-pagination class="my-4" />

            <ais-hits>
              <template slot="item" slot-scope="{ item }">
                <nuxt-link
                  :to="
                    localePath({
                      name: 'item-id',
                      params: { id: item.objectID },
                    })
                  "
                >
                  <v-img contain height="150" :src="item._image" />
                </nuxt-link>

                <nuxt-link
                  :to="
                    localePath({
                      name: 'item-id',
                      params: { id: item.objectID },
                    })
                  "
                >
                  <h3 class="my-4">{{ item._title }}</h3>
                </nuxt-link>

                <ul>
                  <li v-for="(tag, index) in item.tags" :key="tag">
                    <!--
                    <nuxt-link
                      :to="
                        localePath({
                          name: 'search',
                          query: {
                            'kunshujo[refinementList][Tags][0]': tag,
                          },
                        })
                      "
                    >
                      <ais-highlight :hit="item" :attribute="'Tags.' + index" />
                    </nuxt-link>
                    -->
                    <ais-highlight :hit="item" :attribute="'tags.' + index" />
                  </li>
                </ul>
              </template>
            </ais-hits>

            <ais-pagination class="my-4" />
          </v-col>
        </v-row>
      </v-container>
    </ais-instant-search>
  </client-only>
</template>

<script>
import algoliasearch from 'algoliasearch/lite'
import { simple } from 'instantsearch.js/es/lib/stateMappings'
import 'instantsearch.css/themes/algolia-min.css'

export default {
  data() {
    return {
      searchClient: algoliasearch(
        '2YF6SCVA5T',
        'db95bf68a1f91fcf529ac820e7768436'
      ),
      routing: {
        stateMapping: simple(),
      },
    }
  },

  head() {
    return {
      title: this.$t('search'),
    }
  },
}
</script>
