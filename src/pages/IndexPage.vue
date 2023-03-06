<template>
  <q-page class="flex flex-center">
    <q-form @submit.prevent="onSubmit" class="q-gutter-md">
      <q-input
        v-model="form.name"
        filled
        label="Name*"
        name="name"
        lazy-rules
        :rules="[(val) => (val && val.length > 0) || 'Name is required']"
      />
      <q-input
        v-model="form.email"
        filled
        label="Email*"
        name="email"
        lazy-rules
        :rules="[
          (val) => (val && val.length > 0) || 'Email is required',
          (val) =>
            /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(form.email) ||
            'Please enter a valid email',
        ]"
      />
      <q-input
        v-model="form.message"
        filled
        label="Message*"
        name="message"
        autogrow
        lazy-rules
        :rules="[(val) => (val && val.length > 0) || 'Message is required']"
      />
      <div>
        <q-btn
          label="Send"
          type="submit"
          color="primary"
          class="q-py-md full-width"
          no-caps
        />
      </div>
    </q-form>
  </q-page>
</template>

<script>
import { defineComponent, reactive } from "vue";

export default defineComponent({
  name: "IndexPage",
  setup(props, { emit }) {
    const form = reactive({
      name: null,
      email: null,
      phone: null,
      message: null,
    });

    document.addEventListener("chatlio.messageSent", function (event) {
      Object.keys(form).forEach((x) => (form[x] = null));
    });

    const onSubmit = async () => {
      try {
        if (!form.name || !form.email || !form.message) return;

        _chatlio.identify("Petroffsoft", {
          App: "Capella Landing Page",
          Name: form.name,
          Email: form.email,
        });
        _chatlio.show({ expanded: true }).send(form.message);
      } catch (error) {
        console.log(error);
      }
    };

    return {
      form,
      onSubmit,
    };
  },
});
</script>
