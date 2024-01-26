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
  <div>
    <exo-drawer
      id="externalVisioConnectorAddDrawer"
      ref="externalVisioConnectorAddDrawer"
      right
      allow-expand
      @expand-updated="expanded = $event"
      @closed="close">
      <template slot="title">
        <div class="d-flex">
          <v-icon
            size="16"
            class="clickable"
            @click="close()">
            fas fa-arrow-left
          </v-icon>
          <span> {{ $t('externalVisio.drawer.addVisioConnector.title') }} </span>
        </div>
      </template>
      <template slot="content">
        <v-form
          v-model="isValidInputs">
          <v-card-text class="d-flex pb-2">
            <v-label>
              <span class="text-color font-weight-bold text-start text-truncate-2">
                {{ $t('externalVisio.label.nameConnector.title') }}            
              </span>
              <p class="caption">{{ $t('externalVisio.label.nameConnector.description') }}  </p>
            </v-label>
          </v-card-text>
          <v-card-text class="d-flex py-0">
            <v-text-field
              v-model="externalVisioName"
              class="pt-0"
              type="text"
              required="required"
              :placeholder="placeholder"
              outlined
              dense>
              <template #append>
                <v-btn
                  class="mt-n2 pt-2px"
                  icon
                  @click="openTranslationDrawer">
                  <v-icon color="primary">fas fa-language</v-icon>
                </v-btn>
              </template>
            </v-text-field>
          </v-card-text>
          <v-card-text class="d-flex flex-row pt-8">
            <v-label>
              <span class="text-color font-weight-bold">
                {{ $t('externalVisio.label.moduleConnector.title') }}            
              </span>
              <p class="caption"> {{ $t('externalVisio.label.moduleConnector.description') }} </p>
            </v-label>
          </v-card-text>
          <v-card-text class="d-flex flex-row">
            <v-label>
              <span class="text-color">
                {{ $t('externalVisio.label.userModuleConnector.title') }}
              </span>
              <p class="caption">{{ $t('externalVisio.label.userModuleConnector.description') }}</p>
            </v-label>
            <v-spacer />
            <v-switch      
              v-model="isUserConnector"
              class="mt-0 pt-0" />
          </v-card-text>
          <v-card-text class="d-flex flex-row">
            <v-label>
              <span class="text-color">
                {{ $t('externalVisio.label.spaceModuleConnector.title') }}
              </span>
              <p class="caption">{{ $t('externalVisio.label.spaceModuleConnector.description') }}</p>
            </v-label>
            <v-spacer />
            <v-switch      
              v-model="isSpaceConnector"
              class="mt-0 pt-0" />
          </v-card-text>
        </v-form>
      </template>
      <template slot="footer">
        <div class="d-flex justify-end">
          <v-btn
            class="btn ms-2"
            @click="close">
            {{ $t('externalVisio.label.btn.cancel') }}
          </v-btn>
          <v-btn
            :disabled="disabled"
            :loading="loading"
            @click="saveExternalVisioConnector"
            class="btn btn-primary ms-2">
            {{ $t('externalVisio.label.btn.Save') }}
          </v-btn>
        </div>
      </template>
    </exo-drawer>
  </div>
</template>
<script>
export default {
  data () {
    return {
      placeholder: 'Enter the name of the web conferencing',
      externalVisioName: '',
      isUserConnector: true,
      isSpaceConnector: true,
      isValidInputs: true,
    };
  },
  created() {
    this.$root.$on('open-external-visio-add-drawer', this.open);
  },
  computed: {
    disabled() {
      return !(this.isValidInputs && this.externalVisioName);
    },
  },
  methods: {
    open() {
      this.$nextTick().then(() => this.$refs.externalVisioConnectorAddDrawer.open());
    },
    close() {
      this.externalVisioName = null;
      this.$refs.externalVisioConnectorAddDrawer.close();
    },
    saveExternalVisioConnector() {
      this.$externalVisioConnectorService.saveExternalVisioConnector(this.externalVisioName, this.isUserConnector, this.isSpaceConnector)
        .then(() => {
          this.$root.$emit('refresh-external-visio-connectors');
          this.close();
        });
    }
  }
};
</script>