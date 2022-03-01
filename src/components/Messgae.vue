<template>
  <div>
    <v-snackbar v-model="showMessage" rounded="pill" :color="messageColor" :timeout="timeout">
      {{ message }}

      <template v-slot:action="{ attrs }">
        <v-btn color="pink" text v-bind="attrs" @click="showMessage = false">
          Close
        </v-btn>
      </template>
    </v-snackbar>
  </div>
</template>

<script>
export default {
    props: {
        message
    }
}
</script>