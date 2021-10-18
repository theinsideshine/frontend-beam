<template>
  <div class="hello">
    <b>{{ msg }}</b>
    <p style="color:blue" > <b>Version de firwmare :</b> {{ version }}
       <button v-on:click="getVersion" style="color:blue">Get version</button>       
       
    <p/>
    <p> <b> Parametros del ensayo </b>    
      <button v-on:click="getAll" style="color:blue">Get parametros</button>
      <button v-on:click="putStart"  style="color:green">Empezar ensayo</button>
     </p>
    <p style="color:red"><b>Distancia en milimetros :</b>
    <input v-model.number="distance"  style="color:red" type="number" placeholder="Ingrese la distancia." />
    <button v-on:click="putDistance" style="color:red">Set distance</button>
    
    </p>
    <p style="color:red" ><b>Fuerza aplicada en gramos :</b>
    <input v-model.number="force" style="color:red" type="number" placeholder="Ingrese la fuerza." />
    <button v-on:click="putForce" style="color:red">Set fuerza</button>
    
    </p>
     

    <p style="color:blue"> <b>Fuerza de reaccion 1 :</b> {{ reaction_one }}<p/>
    <p style="color:blue"><b>Fuerza de reaccion 2 :</b> {{ reaction_two }}<p/>
    <p style="color:blue"><b>Flexion : </b>{{ flexion }}<p/>
    <p style="color:blue"><b>Status del ensayo:</b> {{ st_test }}</p>

    <p>
     <b style="color:red" >Ingrese ip y puerto del servidor </b>:
      <input v-model="ip_puerto" type="text" style="color:red" placeholder="xxx.xxx.xxx.xxx:xxxx" />
    </p>

    <p> <b> Parametros de configuracion </b>
        <button v-on:click="getAllCal" style="color:blue">Get configuracion</button>       
    </p>

    <p style="color:blue"> <b>Log level :</b> {{ log_level }}
       <b>Cantidad de pasos :</b> {{ step_cal }}
       <b style="color:red">Constante de flexion :</b>
       <input v-model.number="step_k" style="color:red" type="number" placeholder="Ingrese la constante." />
       <button type="button" v-on:click="putStepK" style="color:red">Set flexion_k</button>
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
      log_level: 0,
      step_cal : 100,
      step_k:  0.000123
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
    putStepK () {
      const path = this.ip_puerto + '/parameters/step_k/' + this.step_k
      axios.put(path).then((respuesta) => {
        this.step_k= respuesta.data.step_k
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
    },
    getAllCal () {
      const path = this.ip_puerto + '/info/calibration'
      axios.get(path).then((respuesta) => {       
        this.log_level = respuesta.data.log_level
        this.step_cal= respuesta.data.step_cal
        this.step_k = respuesta.data.step_k
        
      })
        .catch((error) => {
          console.log(error)
        })
    }
  }
}
</script>
