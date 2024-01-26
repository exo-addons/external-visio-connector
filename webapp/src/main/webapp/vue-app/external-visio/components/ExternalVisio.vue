<!--
Copyright (C) 2024 eXo Platform SAS.

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU Affero General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU Affero General Public License for more details.

You should have received a copy of the GNU Affero General Public License
along with this program. If not, see <http://www.gnu.org/licenses/>.
-->
<template>
  <v-app id="ExternalVisioAdmin">
    <h4 class="text-color mb-0">{{ $t('externalVisio.admin.settings.title') }}</h4>
    <span class="caption text-light-color">{{ $t('externalVisio.admin.settings.description') }}</span>
    <external-visio-toolbar />
    <v-data-table
      :headers="headers"
      :items="externalVisioConnectors"
      :items-per-page="itemsPerPage"
      :hide-default-footer="hideFooter"
      :footer-props="{
        itemsPerPageText: `${$t('externalVisio.settings.itemsPerPage')}:`,
      }"
      :loading-text="$t('externalVisio.settings.loadingResults')"
      :no-results-text="$t('externalVisio.settings.noResultsFound')"
      :no-data-text="$t('externalVisio.settings.noExternalVisio')"
      disable-sort>
      <template slot="item" slot-scope="props">
        <tr>
          <td>
            <div :title="props.item.name" class="text-truncate font-weight-bold">
              {{ $t(props.item.name) }}
            </div>
          </td>
          <td>
            <div class="pull-right">
              <v-tooltip top>
                <template #activator="{ on, attrs }">
                  <v-btn 
                    v-if="displayArrowUp(props.item)"
                    icon
                    small
                    v-bind="attrs"
                    v-on="on"
                    @click="moveUp(props.item)">
                    <v-icon
                      size="18"
                      class="primary--text">
                      {{ 'fa-arrow-up' }}
                    </v-icon>
                  </v-btn>
                </template>
                <span>{{ $t('externalVisio.settings.moveup') }}</span>
              </v-tooltip>
              <v-tooltip top>
                <template #activator="{ on, attrs }">
                  <v-btn 
                    v-if="displayArrowDown(props.item)"
                    icon
                    small
                    v-bind="attrs"
                    v-on="on"
                    @click="moveDown(props.item)">
                    <v-icon
                      size="18"
                      class="primary--text">
                      {{ 'fa-arrow-down' }}
                    </v-icon>
                  </v-btn>
                </template>
                <span>{{ $t('externalVisio.settings.movedown') }}</span>
              </v-tooltip>
            </div>
          </td>
          <td>
            <div class="align-center">
              <v-btn 
                primary
                icon>
                <i class="uiIconEdit"></i>
              </v-btn>
            </div>
          </td>
        </tr>
      </template>
    </v-data-table>
    <external-visio-drawer />
  </v-app>
</template>
<script>
export default {
  data: () => ({
    headers: [],
    externalVisioConnectors: [],
    itemsPerPage: 10,
    hideFooter: false,
  }),
  created() {
    this.$root.$on('refresh-external-visio-connectors', this.getExternalVisioConnectors);
    this.getExternalVisioConnectors();
    this.headers = [
      { text: this.$t('externalVisio.settings.name'), align: 'left' },
      { text: this.$t('externalVisio.settings.order'), align: 'right' , width: '60px'},
      { text: this.$t('externalVisio.settings.edit'), align: 'center' , width: '60px'}
    ];
  },
  methods: {
    getExternalVisioConnectors() {
      this.$externalVisioConnectorService.getExternalVisioConnectors()
        .then((connectors) => {
          this.externalVisioConnectors = connectors ;
        });
    },
    displayArrowDown(item) {
      return this.externalVisioConnectors.indexOf(item) !== this.externalVisioConnectors.length-1 ;
    },
    displayArrowUp(item) {
      return this.externalVisioConnectors.indexOf(item) !== 0;
    },
    moveUp(item) {
      const index = this.externalVisioConnectors.indexOf(item);
      this.$set(this.externalVisioConnectors, index , this.externalVisioConnectors[index-1]);
      this.$set(this.externalVisioConnectors, index-1, item);
    },
    moveDown(item) {
      const index = this.externalVisioConnectors.indexOf(item);
      this.$set(this.externalVisioConnectors, index, this.externalVisioConnectors[index+1]);
      this.$set(this.externalVisioConnectors, index + 1, item);
    },
  }
};

</script>