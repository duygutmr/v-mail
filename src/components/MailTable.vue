<template>
  <table class="mail-table">
    <tbody>
      <tr
        v-for="email in unArchieved"
        :key="email.id"
        :class="['clickable', email.read ? 'read' : '']"
        @click="openEmail(email)"
      >
        <td>
          <input type="checkbox" />
        </td>
        <td>{{ email.from }}</td>
        <td>
          <p>
            <strong> {{ email.subject }}</strong>
          </p>
        </td>
        <td class="date">
          {{ format(new Date(email.sentAt), "MMM do yyyy") }}
        </td>
        <td>
          <button @click="archievedEmail(email)">Archieve</button>
        </td>
      </tr>
    </tbody>
  </table>
  <ModalView v-if="openedEmail" @closeModal="openedEmail = null">
    <MailView :email="openedEmail" />
  </ModalView>
</template>

<script>
import { format } from "date-fns";
import { ref } from "vue";
import axios from "axios";
import MailView from "./MailView.vue";
import ModalView from "./ModalView.vue";

export default {
  async setup() {
    let { data: emails } = await axios.get("http://localhost:3000/emails");
    // let emails =  response.data
    return {
      format,
      emails,
      openedEmail: null,
    };
  },
  components: {
    MailView,
    ModalView,
  },
  computed: {
    sortedEmails() {
      return this.emails.sort((e1, e2) => {
        return e1.sentAt < e2.sentAt ? 1 : -1;
      });
    },
    unArchieved() {
      return this.sortedEmails.filter((e) => !e.archived);
    },
  },
  methods: {
    openEmail(email) {
      email.read = true;
      this.updateEmail(email);
      this.openedEmail = email;
    },
    archievedEmail(email) {
      email.archived = true;
      this.updateEmail(email);
    },
    updateEmail(email) {
      axios.put(`http://localhost:3000/emails/${email.id}`, email);
    },
  },
};
</script>

<style>
</style>