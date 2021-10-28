/* eslint-disable */
<template>
  <div class = "users">
    <div class="container">
      <section> 
        <h4 class="title">Nova Consulta</h4>
        <form @submit.prevent="CreatePlano">
          <input type="text" placeholder="Origem" v-model="form.origem">
          <input type="text" placeholder="Destino" v-model="form.destino">
          <input type="number" placeholder="Tempo" v-model="form.tempo">
          <input type="text" placeholder="Plano" v-model="form.nome">
          <button type="submit">Consultar</button>
        </form>
      </section>

      <section>
        <h5 class="title">Lista de Planos</h5>
        <table style="width:100%">
            <tr>
              <th>Origem</th>
              <th>Destino</th>
              <th>Tempo</th>
              <th>Plano FaleMais</th>
              <th>Com FaleMais</th>
              <th>Sem FaleMais</th>
            </tr>
            <tr v-for="plano in planos" :key="plano.id">
              <td>{{plano.origem}}</td>
              <td>{{plano.destino}}</td>
              <td>{{plano.tempo}}</td>
              <td>{{plano.nome}}</td>
              <td>{{plano.valorComPlano}}</td>
              <td>{{plano.valorSemPlano}}</td>
              <!--<td><a class="destroy"></a></td>-->
              
            </tr>
          </table>
      </section>
    </div>
  </div>
</template>

<script lang="ts">
/* eslint-disable */
import { defineComponent } from "vue";
import axios from '@/utils/axios'

interface Plano{
  origem: string
  destino: string
  tempo: number
  nome: string
  valorComPlano: number
  valorSemPlano: number
}

export default defineComponent({
  data() {
    return {
      planos: [] as Plano[],
      form: {
        origem: '',
        destino: '',
        tempo: 1,
        nome: ''
      }
    }
  },
  async created(){
    await this.fetchPlanos();
    console.log(this.planos)
  }, 
  methods : {
    async fetchPlanos() {
      try {
        const { data }  = await axios.get('/api/planos')
        this.planos = data;
      } catch (error) {
        console.warn(error)
      }
    },
    async CreatePlano(){
      try {
        const { data } = await axios.post('/api/planos', this.form)
        this.planos.push(data)
        this.form.nome = ''
        this.form.origem = ''
        this.form.destino = ''
        this.form.tempo = 0   
      } catch (error) {
        console.warn(error)
      }
    }
  }
});
</script>

<style scoped>
table, th, td {
  border: 1px solid black;
  border-collapse: collapse;
  text-align: center;
}

.container {
  margin: 4rem auto;
  max-width: 500px;
  width: 90%;
  display: grid;
  grid-gap: 2.5rem;
}

.title {
  font-size: 1rem;
  font-weight: 500;
  margin: 0.7rem 0;
}

form {
  display: grid;
  grid-gap: 1rem;
}

input {
  background: transparent;
  border: 1px solid #999fc6;
  border-radius: 1rem;
  padding: 0.6rem;
  outline: none;
  color: #e1e8ef;
}

input::placeholder {
  color: #999fc6;
}

button {
  background-color: #2d6cea;
  color: #e1e8ef;
  border: none;
  border-radius: 1rem;
  padding: 0.6rem 1.5rem;
  width: max-content;
  transition: all 0.3s linear;
  outline: none;
  cursor: pointer;
  box-shadow: 0 0 5px 3px rgba(45, 108, 234, 0.3);
}

button:hover {
  background-color: #1b5cdc;
}

p {
  margin: 0;
}

ul {
  padding: 0;
  margin: 0;
  display: grid;
  grid-gap: 1rem;
}

li {
  background-color: #2b3a4e;
  padding: 1.2rem 1rem;
  border-radius: 1rem;
  position: relative;
  list-style: none;
  color: #8b98a8;
}

.destroy {
  background-color: #d53e6b;
  width: 24px;
  height: 24px;
  border-radius: 0.5rem;
  cursor: pointer;
  transition: all 0.2s linear;
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  right: 1.3rem;
}

.destroy:before,
.destroy:after {
  content: '';
  width: 3px;
  height: 13px;
  background-color: #ececf6;
  border-radius: 1rem;
  position: absolute;
  top: 50%;
  left: 50%;
}

.destroy:before {
  transform: translate(-50%, -50%) rotate(45deg);
}

.destroy:after {
  transform: translate(-50%, -50%) rotate(130deg);
}

.destroy:hover {
  background-color: #984848;
}
</style>