<template>
  <div class="Principal">
    <div class="detalles">
      <h3
        :style="
          alerta === 'Tarea agregada correctamente'
            ? 'color:green'
            : 'color:red'
        "
      >
        {{ alerta }}
      </h3>
      <label for="activity">Actividad</label>
      <input
        type="text"
        class="form"
        name="activity"
        id="activity"
        placeholder="Ingrese su tarea"
        v-model="activity"
      />
      <label for="check">Prioridad</label>
      <input
        type="checkbox"
        class="form"
        name="check"
        id="check"
        v-model="check"
      />
      <input
        type="date"
        class="form"
        name="date_register"
        id="date_register"
        placeholder="Fecha de la tarea"
        v-model="date"
      />
    </div>
    <button @click="agregar()">+</button>
    <button @click="ordenar()">Ordenar</button>

    <table>
      <thead>
        <tr>
          <th>Actividad</th>
          <th>Prioridad</th>
          <th>Fecha</th>
          <th>Opciones</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="(item, i) in registros"
          :key="i"
          :style="{
            backgroundColor: item.priority === 'Alta' ? 'red' : '',
            color: item.priority === 'Alta' ? 'white' : '',
          }"
        >
          <td>{{ item.activity }}</td>
          <td>{{ item.priority }}</td>
          <td>{{ item.date }}</td>
          <td>
            <button @click="borrar(i)">Borrar</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref } from "vue";

const registros = ref([]);
const activity = ref("");
const check = ref(false);
const date = ref("");
const alerta = ref("");

const ocultarAlerta = () => {
  setTimeout(() => {
    alerta.value = "";
  }, 2500);
};

function borrar(i) {
  registros.value.splice(i, 1);
}

const agregar = () => {
  if (activity.value === "") {
    alerta.value = "Ingrese una actividad";
    ocultarAlerta();
  } else if (date.value === "") {
    alerta.value = "Ingrese una fecha";
    ocultarAlerta();
  } else {
    alerta.value = "Tarea agregada correctamente";
    ocultarAlerta();
    registros.value.push({
      activity: activity.value,
      priority: check.value ? "Alta" : "Baja",
      date: date.value,
    });
    limpiar();
    console.log(registros.value);
  }
};

function limpiar() {
  activity.value = "";
  check.value = "";
  date.value = "";
}

const ordenar = () => {
  registros.value.sort((a, b) => {
    return a.priority === "Alta" ? -1 : 1;
  });
};
</script>
<style>
body {
  background-color: #366240;
  margin: 0;
  padding: 0;
}

.Principal {
  max-width: 800px;
  margin: 0 auto;
  font-family: Arial, sans-serif;
  background-color: #cae0e7;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.detalles {
  margin-bottom: 20px;
}

label {
  display: block;
  margin-bottom: 5px;
}

.form {
  width: 100%;
  padding: 8px;
  margin-bottom: 10px;
  box-sizing: border-box;
}

input[type="checkbox"] {
  margin-right: 5px;
}

button {
  padding: 10px;
  margin-right: 10px;
  cursor: pointer;
  background-color: #3498db;
  color: #ffffff;
  border: none;
  border-radius: 5px;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #79afd3;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

th,
td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

th {
  background-color: #f2f2f2;
}

tbody tr:nth-child(even) {
  background-color: #f9f9f9;
}

h3 {
  margin-top: 20px;
  color: #27ae60;
}

input[type="date"] {
  width: 100%;
  padding: 8px;
  margin-bottom: 10px;
  box-sizing: border-box;
}
</style>
