<template>
  <v-app>
    <div class="full-width">
      <v-form ref="formRef" v-model="valid" lazy-validation>
        <v-text-field
          v-model="name"
          :rules="nameRules"
          label="Vollständiger Name"
          required
        ></v-text-field>

        <v-text-field
          v-model="email"
          :rules="emailRules"
          label="E-mail"
          required
        ></v-text-field>

        <v-text-field
          v-model="amount"
          :rules="amountRules"
          label="Betrag (€)"
          type="number"
          required
        ></v-text-field>

        <v-btn
          :disabled="!valid"
          color="success"
          class="mr-4"
          @click="submit"
        >
          Spenden
        </v-btn>
      </v-form>
    </div>
  </v-app>
</template>

<script lang="ts">
import { ref, toRefs } from 'vue';

export default {
  setup() {
    const valid = ref(true);
    const name = ref("");
    const email = ref("");
    const amount = ref("");
    const formRef = ref(null);

    const nameRules = [
      (v: string) => !!v || "Name ist erforderlich",
      (v: string) => (v && v.length >= 3) || "Name muss mindestens 3 Zeichen lang sein"
    ];

    const emailRules = [
      (v: string) => !!v || "E-mail ist erforderlich",
      (v: string) => /.+@.+\..+/.test(v) || "E-mail muss gültig sein"
    ];

    const amountRules = [
      (v: string) => !!v || "Betrag ist erforderlich",
      (v: number) => v > 0 || "Betrag muss größer als 0 sein"
    ];

    const submit = () => {
      if (formRef.value?.validate()) {
        alert("Vielen Dank für Ihre Spende von €" + amount.value);
        name.value = "";
        email.value = "";
        amount.value = "";
        formRef.value?.reset();
      }
    };

    return {
      ...toRefs({valid, name, email, amount, formRef}),
      nameRules,
      emailRules,
      amountRules,
      submit
    };
  }
};
</script>

<style scoped>
.full-width {
  width: 100vw;
}
</style>
