<template>
  <v-form
  ref="form"
  v-model="valid"
  lazy-validation>
  <v-layout wrap>
    <v-flex xs12 sm4>
      <Panel title="Editar Canción">
      <v-text-field
      required
      :rules="required"
      v-model="song.titulo"
      label="Titulo">
      </v-text-field>
      <v-text-field
      required
      :rules="required"
      label="Artista"
      v-model="song.artista">
      </v-text-field>
      <v-text-field
      required
      :rules="required"
      label="Genero"
      v-model="song.genero">
      </v-text-field>
      <v-text-field
      required
      :rules="required"
      label="Album"
      v-model="song.album">
      </v-text-field>
      <v-text-field
      required
      :rules="required"
      label="Url imagen"
      v-model="song.albumImagenUrl">
      </v-text-field>
      <v-text-field
      required
      :rules="required"
      label="Youtube ID"
      v-model="song.youtubeId">
      </v-text-field>
      </Panel>
    </v-flex>
    <v-flex xs12 sm8>
      <Panel  title="Estructura de la Canción" class="ml-2">
        <v-textarea
        required
        rows="6"
        :rules="required"
        label="Letra"
        v-model="song.letra">
        </v-textarea>
        <v-textarea
        required
        rows="7"
        :rules="required"
        label="Tab"
        v-model="song.tab">
        </v-textarea>
        <v-btn color="teal lighten-2" @click="guardar" class="white--text">Guardar</v-btn>
      </Panel>
        <v-alert
        :value="err"
         color="error">
        {{ err }}
        </v-alert>
  </v-flex>
  </v-layout>
  </v-form>
</template>

<script>
import SongsService from '@/services/SongsService'
import Panel from '@/components/Panel'
import { mapState } from 'vuex'
export default {
  name: 'createSong',
  data () {
    return {
      valid: true,
      song: {
        titulo: null,
        artista: null,
        genero: null,
        album: null,
        albumImagenUrl: null,
        youtubeId: null,
        letra: null,
        tab: null,
        UserId: null
      },
      required: [(value) => !!value || 'Campo requerido.'],
      err: null
    }
  },
  computed: {
    ...mapState([
      'user'
    ])
  },
  methods: {
    async guardar () {
      this.err = null
      // const allFilled = Object.keys(this.song).every((key) => !!this.song[key])
      if (this.$refs.form.validate()) {
        try {
          await SongsService.put(this.song)
          this.$router.push({name: 'song', params: {songId: this.song.id}})
        } catch (err) {
          this.err = err.response.data.error
        }
      }
    }
  },
  async mounted () {
    const songId = this.$route.params.songId
    this.song = (await SongsService.show(songId)).data
    console.log(this.song)
  },
  components: {
    Panel
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
