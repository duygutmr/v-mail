<template>
  <div class="email-display">
    <h2 class="mb-0">
      Subject: <strong>{{ email.subject }}</strong>
    </h2>
    <div>
      <em>
        {{ email.from }} on {{ format(new Date(email.sentAt), "MMM do yyyy") }}
      </em>
    </div>
    <div v-html="marked(email.body)"></div>
  </div>
</template>

<script>
import { format } from "date-fns";
import  marked from "marked";
export default {
  setup() {
    return {
      format,
      marked
    };
  },
  props: {
    email: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style>
</style>