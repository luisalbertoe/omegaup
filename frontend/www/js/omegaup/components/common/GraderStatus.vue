<template>
  <ul>
    <hr class="dropdown-separator" />
    <li class="grader grader-submissions">
      <a class="grader-submissions-link" href="/arena/admin/">{{
        T.wordsLatestSubmissions
      }}</a>
    </li>
    <li class="grader grader-status">{{ graderStatusMessage }}</li>
    <li class="grader grader-broadcaster-sockets" v-if="status === 'ok'">
      Broadcaster sockets:
      {{ graderInfo !== null ? graderInfo.broadcaster_sockets : '' }}
    </li>
    <li class="grader grader-broadcaster-sockets" v-else-if="error !== null">
      API api/grader/status call failed:
      <pre style="width: 40em;">{{ error }}</pre>
    </li>
    <li class="grader grader-embedded-runner" v-if="status === 'ok'">
      Embedded runner:
      {{ graderInfo !== null ? graderInfo.embedded_runner : '' }}
    </li>
    <li class="grader grader-queues" v-if="status === 'ok'">
      Queues:
      <pre
        style="width: 50em;"
        v-if="graderInfo !== null"
        v-html="UI.prettyPrintJSON(graderInfo.queue)"
      ></pre>
    </li>
  </ul>
</template>

<style>
.grader-submissions,
a.grader-submissions-link {
  background-color: #fff;
  color: #000 !important;
  text-decoration: none;
}

.grader-submissions:hover,
a.grader-submissions-link:hover {
  background-color: #fff !important;
}

.grader {
  padding: 3px 20px;
}

hr.dropdown-separator {
  margin: 0;
}
</style>

<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator';
import { T } from '../../omegaup.js';
import UI from '../../ui.js';
import omegaup from '../../api.js';

@Component({})
export default class GraderStatus extends Vue {
  @Prop() status!: string;
  @Prop() error!: string;
  @Prop() graderInfo!: omegaup.Grader;

  T = T;
  UI = UI;

  get graderStatusMessage(): string {
    return this.status === 'ok' ? 'Grader OK' : 'Grader DOWN';
  }
}
</script>
