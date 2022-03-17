<template>
  <form>
    <div>
      <label>Activités</label>
      <div>
        <button @click="push({ name: '', good: false })" type="button">
          Ajouter une activité
        </button>
      </div>
    </div>
    <div v-for="(field, i) in fields" :key="field.key">
      <fieldset
        class="InputGroup"
        v-for="(field, idx) in fields"
        :key="field.key"
      >
        <legend>User #{{ idx }}</legend>
        <label :for="`name_${idx}`">Name</label>
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

      <button type="button" @click="push({ email: '', name: '' })">
        Add User +
      </button>
      <br />
      <br />
    </div>
    <pre>{{ values }}</pre>
  </form>
</template>

<script setup lang="ts">
import { useForm, useFieldArray, Field } from 'vee-validate';
import { z } from 'zod';
import { toFormValidator } from '@vee-validate/zod';

const validationSchema = toFormValidator(
  z.object({
    hobbies: z
      .array(
        z.object({
          name: z
            .string({ required_error: 'obligatoire' })
            .min(5, { message: 'trop court !' }),
          comment: z.string(),
        })
      )
      .optional(),
  })
);

const { values } = useForm({ validationSchema });
const { fields, push, remove } = useFieldArray('hobbies');
</script>

<style scoped lang="scss"></style>
