<template>
  <div class="panel panel-default">
    <div class="panel-body">
      <div class="upload-csv">
        <div class="panel-heading">
          <div v-html="T.groupsCsvHelp"></div>
          {{ T.groupsUploadCsvFile }}
          <input name="identities" type="file" v-on:change="readCsv" />
        </div>
      </div>
      <br />
      <div
        class="panel panel-default no-bottom-margin"
        v-show="identities.length &gt; 0"
      >
        <div class="panel-heading">
          <h3 class="panel-title">{{ T.wordsIdentities }}</h3>
        </div>
        <table class="identities-table table">
          <thead>
            <tr>
              <th>{{ T.profileUsername }}</th>
              <th>{{ T.profile }}</th>
              <th>{{ T.loginPassword }}</th>
              <th>{{ T.profileCountry }}</th>
              <th>{{ T.profileState }}</th>
              <th>{{ T.wordsGender }}</th>
              <th>{{ T.profileSchool }}</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="identity in identities">
              <td class="username">
                <strong>{{ identity.username }}</strong>
              </td>
              <td>{{ identity.name }}</td>
              <td class="password">{{ identity.password }}</td>
              <td>{{ identity.country_id }}</td>
              <td>{{ identity.state_id }}</td>
              <td>{{ identity.gender }}</td>
              <td>{{ identity.school_name }}</td>
            </tr>
          </tbody>
        </table>
        <div class="panel-heading">
          <button
            class="btn btn-primary"
            name="create-identities"
            v-on:click.prevent="$emit('bulk-identities', identities)"
          >
            {{ T.groupCreateIdentities }}
          </button>
        </div>
        <div class="panel-footer">
          <button
            class="btn"
            v-on:click.prevent="$emit('download-identities', identities)"
          >
            <span
              class="glyphicon glyphicon-download-alt"
              aria-hidden="true"
            ></span>
          </button>
          {{ T.groupsIdentityWarning }}
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Vue, Component, Prop, Emit } from 'vue-property-decorator';
import { omegaup } from '../../omegaup';
import T from '../../lang';
import * as ui from '../../ui';

@Component
export default class Identities extends Vue {
  @Prop() groupAlias!: string;

  T = T;
  identities: omegaup.Identity[] = [];

  readCsv(ev: InputEvent): void {
    const fileUpload = <HTMLInputElement>ev.target;
    this.identities = [];
    if (fileUpload.value == '') {
      return;
    }
    const regex = /.*\.(?:csv|txt)$/;

    if (!regex.test(fileUpload.value.toLowerCase())) {
      ui.error(T.groupsInvalidCsv);
      return;
    }
    this.$emit('read-csv', this, fileUpload);
  }
}
</script>
