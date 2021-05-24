<template>
  <div class="hello">
    {{ msg }}
    <p>Distancia en milimetros :
    <input v-model.number="distance" type="number" placeholder="Ingrese la distancia." />
    <button v-on:click="putDistance">Put distance</button>
    </p>
    <p>Fuerza aplicada en gramos :
    <input v-model.number="force" type="number" placeholder="Ingrese la fuerza." />
    <button v-on:click="putForce">Put fuerza</button>
    <button v-on:click="putStart">Empezar ensayo</button>

    </p>
     <p>Version : {{ version }}
       <button v-on:click="getVersion">Get version</button>
       <button v-on:click="getAll">Get parametros</button>
      <p/>
     <p>Status : {{ st_test }}
       <button v-on:click="getStatus">Get status</button>
       Log level : {{ log_level }}
      <p/>
      <p>Fuerza de reaccion 1 : {{ reaction_one }}
       <button v-on:click="getReaction_one">Get reaction1</button>
      <p/>
      <p/>
      <p>Fuerza de reaccion 2 : {{ reaction_two }}
       <button v-on:click="getReaction_two">Get reaction2</button>
      <p/>
      <p>Flexion : {{ flexion }}
       <button v-on:click="getFlexion">Get flexion</button>
      <p/>
      <p>Ingrese ip y puerto del servidor :
    <input v-model="ip_puerto" type="text" placeholder="xxx.xxx.xxx.xxx:xxxx" />
    </p>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data () {
    return {
      ip_puerto: 'http://192.168.0.103:4000',
      version: 'Sin version',
      st_test: 'Sin status',
      reaction_one: 'Sin fuerza R1',
      reaction_two: 'Sin fuerza R2',
      flexion: 'Sin distancia de flexion',
      distance: 0,
      force: 0,
      log_level: 0
    }
  },
  methods: {
    putDistance () {
      const path = this.ip_puerto + '/parameters/distance/' + this.distance
      axios.put(path).then((respuesta) => {
        this.distance = respuesta.data.distance // Obtiene el valor del al key="distance" de la respuesta Inconsitencia peligrosa
      })
        .catch((error) => {
          console.log(error)
        })
    },
    putForce () {
      const path = this.ip_puerto + '/parameters/force/' + this.force
      axios.put(path).then((respuesta) => {
        this.force = respuesta.data.force
      })
        .catch((error) => {
          console.log(error)
        })
    },
    putStart () {
      this.st_test = 1
      const path = this.ip_puerto + '/comands/start'
      axios.put(path).then((respuesta) => {
      // Aca manda st_test TI
        this.st_test = respuesta.data.st_test
      })
        .catch((error) => {
          console.log(error)
        })
    },
    getAll () {
      const path = this.ip_puerto + '/info/parameters'
      axios.get(path).then((respuesta) => {
        // La respuesta no es la misma que cuando se pide solo el st_test. Inconsitencia peligrosa
        this.st_test = respuesta.data.st_test
        this.reaction_one = respuesta.data.reaction_one
        this.reaction_two = respuesta.data.reaction_two
        this.flexion = respuesta.data.flexion
        this.distance = respuesta.data.distance
        this.force = respuesta.data.force
        this.log_level = respuesta.data.log_level
      })
        .catch((error) => {
          console.log(error)
        })
    },
    getStatus () {
      const path = this.ip_puerto + '/info/status'
      axios.get(path).then((respuesta) => {
        // La respuesta no es la misma que cuando se pide info/parameters el st_test   Inconsitencia peligrosa
        this.st_test = respuesta.data.status
      })
        .catch((error) => {
          console.log(error)
        })
    },
    getVersion () {
      const path = this.ip_puerto + '/info/version'
      axios.get(path).then((respuesta) => {
        this.version = respuesta.data.version
      })
        .catch((error) => {
          console.log(error)
        })
    },
    getReaction_one () {
      const path = this.ip_puerto + '/info/reaction_one'
      axios.get(path).then((respuesta) => {
        this.reaction_one = respuesta.data.reaction_one
      })
        .catch((error) => {
          console.log(error)
        })
    },
    getReaction_two () {
      const path = this.ip_puerto + '/info/reaction_two'
      axios.get(path).then((respuesta) => {
        this.reaction_two = respuesta.data.reaction_two
      })
        .catch((error) => {
          console.log(error)
        })
    },
    getFlexion () {
      const path = this.ip_puerto + '/info/flexion'
      axios.get(path).then((respuesta) => {
        this.flexion = respuesta.data.flexion // key = "flexion"
      })
        .catch((error) => {
          console.log(error)
        })
    }
  }
}
</script>
