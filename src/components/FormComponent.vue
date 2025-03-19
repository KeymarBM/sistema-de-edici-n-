<template>
    <div class="form-container">
      <div class="form-group">
        <label>Nombre</label>
        <input ref="nombre" type="text" v-model="persona.nombre">
      </div>
      <div class="form-group">
        <label>Apellido</label>
        <input type="text" v-model="persona.apellido">
      </div>
      <div class="form-group">
        <label>Edad</label>
        <input type="number" v-model="persona.edad">
      </div>
      <div class="button-container">
        <button v-if="!edicion" class="btn-primary" @click="guardar()">GUARDAR</button>
        <button v-if="edicion" class="btn-edit" @click="editar()">ACTUALIZAR</button>
      </div>
  
      <table v-if="personas.length > 0">
        <thead>
          <tr>
            <th>NOMBRE</th>
            <th>APELLIDO</th>
            <th>EDAD</th>
            <th>ACCIÓN</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(data, index) in personas" :key="index">
            <td>{{ data.nombre }}</td>
            <td>{{ data.apellido }}</td>
            <td>{{ data.edad }}</td>
            <td>
              <div class="action-buttons">
                <button class="btn-danger" @click="borrar(index)">Borrar</button> 
                <button class="btn-edit" @click="editando(index)">Editar</button>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
      <div v-else class="empty-message">No hay registros disponibles</div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        personas: [],
        persona: {
          nombre: '',
          apellido: '',
          edad: 0
        },
        editIndex: -1,
        edicion: false
      };
    },
    methods: {
      guardar() {
        if (!this.persona.nombre || !this.persona.apellido) {
          alert('Por favor complete todos los campos');
          return;
        }
        let obj = { ...this.persona };
        this.personas.push(obj);
        this.limpiar();
      },
      
      limpiar() {
        this.persona.nombre = '';
        this.persona.apellido = '';
        this.persona.edad = 0;
        this.$refs['nombre'].focus();
      },
      
      borrar(pos) {
        if (confirm('¿Está seguro de eliminar este registro?')) {
          this.personas.splice(pos, 1);
        }
      },
      
      editando(index) {
        this.edicion = true;
        this.editIndex = index;
        this.persona = { ...this.personas[index] };
        this.$refs['nombre'].focus();
      },
      
      editar() {
        if (this.editIndex !== -1) {
          if (!this.persona.nombre || !this.persona.apellido) {
            alert('Por favor complete todos los campos');
            return;
          }
          this.personas[this.editIndex] = { ...this.persona };
          this.limpiar();
          this.edicion = false;
          this.editIndex = -1;
        }
      }
    },
    mounted() {
      this.$refs['nombre'].focus();
    }
  };
  </script>
  