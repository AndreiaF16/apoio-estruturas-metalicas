<template>
  <form @submit.prevent="upload">
    <!-- Styled -->
    <b-form-file
      v-model="file"
      :state="hasFile"
      placeholder="Choose a file or drop it here..."
      drop-placeholder="Drop file here..."
    ></b-form-file>
    <div class="mt-3">
      Selected file: {{ file ? file.name : '' }}
    </div>

    <nuxt-link :to="`/clients/${id}/${projectId}/file`">Voltar</nuxt-link>

    <b-button type="submit" :disabled="!hasFile">Upload</b-button>
  </form>
</template>

<script>
export default {
  auth: false,
  data() {
    return {
      //username: this.$auth.user.sub,
      file: null
    }
  },

  computed: {
    id() {
        return this.$route.params.id
    },
    projectId() {
        return this.$route.params.projectId
    },
    hasFile () {
      return this.file != null
    },
    formData () {
      let formData = new FormData()

      formData.append('id', this.$route.params.projectId)//this.$auth.user.sub)

      if (this.file) {
        formData.append('file', this.file)
      }

      return formData
    }
  },

  methods: {
    upload() {
      if (!this.hasFile) {
        return
      }

      let promisse = this.$axios.$post('http://localhost:8080/projetoEstruturas/api/files/upload', this.formData, {
        headers: {
          'Content-Type': 'multipart/form-data'
        }
      })

      promisse.then(() => {
        this.$router.push("/clients/"+this.id+"/"+this.projectId+"/file");
        this.$toast.success('File uploaded!').goAway(3000)
      })
    }
  }
}
</script>