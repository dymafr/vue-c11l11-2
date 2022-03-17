<template>
  <form>
    <div v-for="(field, i) in fields" :key="field.key">
      <fieldset
        class="InputGroup"
        v-for="(field, idx) in fields"
        :key="field.key"
      >
        <legend>User #{{ idx }}</legend>
        <label :for="`name_${idx}`">Prénom</label>
        <Field :id="`name_${idx}`" :name="`users[${idx}].name`" />
        <ErrorMessage :name="`users[${idx}].name`" />

        <label :for="`email_${idx}`">Email</label>
        <Field
          :id="`email_${idx}`"
          :name="`users[${idx}].email`"
          type="email"
        />
        <ErrorMessage :name="`users[${idx}].email`" />
        <button type="button" @click="remove(idx)">X</button>
      </fieldset>
    </div>

    <button type="button" @click="push({ email: '', name: '' })">
      Add User +
    </button>
    <br />
    <br />

    <pre>{{ values }}</pre>
  </form>
</template>

<script setup lang="ts">
import { useForm, useFieldArray, Field } from 'vee-validate';
import { z } from 'zod';
import { toFormValidator } from '@vee-validate/zod';

const validationSchema = toFormValidator(
  z.object({
    users: z.array(
      z.object({
        name: z.string({ required_error: 'obligatoire' }).min(1),
        email: z.string().email(),
      })
    ),
  })
);

const { values } = useForm({
  validationSchema,
  initialValues: {
    users: [{ name: '', email: '' }],
  },
});
const { fields, push, remove } = useFieldArray('users');
</script>

<style>
#app {
  font-family: Arial, Helvetica, sans-serif;
  max-width: 500px;
}

input {
  display: block;
}

span {
  display: block;
  margin-bottom: 20px;
}

label {
  display: block;
  margin-top: 20px;
}

button {
  display: block;
}

button[type='submit'] {
  margin-top: 10px;
}

form {
  padding: 20px;
  border: 1px solid black;
}

form + form {
  margin-top: 20px;
}

.InputGroup {
  padding: 10px;
  border: 2px dotted black;
  margin-bottom: 30px;
  position: relative;
}

.InputGroup button {
  position: absolute;
  top: 10px;
  right: 10px;
}
</style>
