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
            <v-row>
              <v-col col="12" sm="6">
                <h2>{{ $t('filter') }}</h2>
              </v-col>
              <v-col col="12" sm="6" class="text-right">
                <ais-clear-refinements>
                  <span slot="resetLabel">{{ $t('reset') }}</span>
                </ais-clear-refinements>
              </v-col>
            </v-row>
            <v-card flat outlined class="mt-4">
              <v-card-title class="headline grey lighten-2">
                {{ $t('Tags') }}
              </v-card-title>
              <v-card-text>
                <ais-refinement-list
                  class="mt-4"
                  show-more
                  operator="and"
                  :show-more-limit="100"
                  :limit="20"
                  attribute="tags"
                />
              </v-card-text>
            </v-card>

            <v-card flat outlined class="mt-4">
              <v-card-title class="headline grey lighten-2">
                {{ $t('Label') }}
              </v-card-title>
              <v-card-text>
                <ais-refinement-list
                  class="mt-4"
                  show-more
                  operator="and"
                  :show-more-limit="100"
                  :limit="20"
                  attribute="label"
                />
              </v-card-text>
            </v-card>
          </v-col>
          <v-col col="12" sm="8">
            <ais-search-box :placeholder="$t('add_a_search_term')" />

            <client-only>
              <ais-powered-by class="my-2" />
            </client-only>

            <v-sheet class="pa-4 my-4" color="grey lighten-3">
              <v-row>
                <v-col>
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
                      {{ $t('search_result') }}:{{ nbHits.toLocaleString() }} {{ $t('hits') }}
                    </p>
                  </ais-stats>
                </v-col>
                <v-col class="text-right">
                  <ais-hits-per-page
                    :items="[
                      { label: '24', value: 24, default: true },
                      { label: '60', value: 60 },
                      { label: '120', value: 120 },
                      { label: '512', value: 512 },
                    ]"
                  />
                </v-col>
              </v-row>
            </v-sheet>

            <ais-pagination :padding="2" class="my-4" />

            <ais-hits>
              <v-row slot-scope="{ items }">
                <v-col
                  v-for="item in items"
                  :key="item.objectID"
                  col="12"
                  sm="3"
                >
                  <v-card flat outlined>
                    <nuxt-link
                      :to="
                        localePath({
                          name: 'item-id',
                          params: { id: item.objectID },
                        })
                      "
                    >
                      <v-img
                        contain
                        max-height="150"
                        style="height: 150px"
                        width="100%"
                        class="grey lighten-2"
                        :src="item.image"
                      />
                    </nuxt-link>

                    <v-card-text>
                      <nuxt-link
                        :to="
                          localePath({
                            name: 'item-id',
                            params: { id: item.objectID },
                          })
                        "
                      >
                        <h3 class="mb-4">{{ item.title }}</h3>
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
                          <ais-highlight
                            :hit="item"
                            :attribute="'tags.' + index"
                          />
                        </li>
                      </ul>
                    </v-card-text>
                  </v-card>
                </v-col>
              </v-row>
            </ais-hits>

            <ais-pagination :padding="2" class="my-4" />
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
