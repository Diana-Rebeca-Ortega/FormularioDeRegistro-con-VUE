<script setup lang="ts">
import { ref } from 'vue'

interface Alumno {
  numControl: string;
 nombre:string;
edad: number | null;
semestre:string;
carrera: string;
materias:string[];
especialidad: string;
extraescolares:[];
}

const listaAlumnos = ref<Alumno[]>([]);
const nuevoAlumno = ref<Alumno>({
  numControl: '',
  nombre: '',
  edad: null,
  semestre: '',
  carrera: '',
  materias: [], // Se llenará con checkboxes
  especialidad: '',
  extraescolares: []
});
//Esta es la funcion para guardar el regustro**********************
const registrar = () => {
  listaAlumnos.value.push({ ...nuevoAlumno.value });
  //Limpiar campos después de registrar
  nuevoAlumno.value = {
    numControl: '', nombre: '', edad: null, semestre: '', 
    carrera: '', materias: [], especialidad: '', extraescolares: []
  };
  alert("¡Alumno registrado con éxito!");
};
const materiasCarrera = {
  'Ingeneria en Sistemas Computacionales': {
    materias: ['Sistemas Operativos', 'POO', 'Taller BDD'],
    especialidades: ['IA', 'Topicos Selectos de Programación']
  },
  'Ing Industrial': {
    materias: ['Cocina 1', 'Fracciones ', 'Pozole 3'],
    especialidades: ['Termodinamica', 'Quimica']
  },
  'Administración': {
    materias: ['Contabilidad', 'Derecho', 'Tiktok'],
    especialidades: ['Instagram', 'Cafeteria 4']
  }
}
const carreras = Object.keys(materiasCarrera);
const actividadesExtraescolares = ref(['Volley', 'Escolta', 'Baile', 'Futbol', 'Moocs']);
</script>

<template>
  <div class="container mt-5">
    <div class="card shadow border-0 mb-5">
      <div class="card-header bg-primary text-white py-3">
        <h2 class="h4 mb-0 text-center">Registro de Inscripción de Alumnos</h2>
      </div>
      <div class="card-body p-4">
        <form @submit.prevent="registrar">
          
          <div class="row g-3 mb-4">
            <h5 class="text-secondary border-bottom pb-2">Información Básica</h5>
            <div class="col-md-6">
              <label class="form-label fw-bold">Número de Control</label>
              <input v-model="nuevoAlumno.numControl" class="form-control" placeholder="Ej: 21210000" required>
            </div>
            <div class="col-md-6">
              <label class="form-label fw-bold">Nombre Completo</label>
              <input v-model="nuevoAlumno.nombre" class="form-control" placeholder="Nombre del alumno" required>
            </div>
            <div class="col-md-6">
              <label class="form-label fw-bold">Edad</label>
              <input v-model.number="nuevoAlumno.edad" type="number" class="form-control" placeholder="Edad">
            </div>
            <div class="col-md-6">
              <label class="form-label fw-bold">Semestre</label>
              <select v-model="nuevoAlumno.semestre" class="form-select">
                <option disabled value="">Selecciona Semestre</option>
                <option v-for="n in 10" :key="n" :value="n">{{ n }}° Semestre</option>
              </select>
            </div>
          </div>

          <div class="row mb-4">
            <div class="col-12">
              <label class="form-label fw-bold">Carrera</label>
              <select v-model="nuevoAlumno.carrera" class="form-select bg-light" @change="nuevoAlumno.materias = []; nuevoAlumno.especialidad = ''">
                <option disabled value="">Elegir carrera...</option>
                <option v-for="carrera in carreras" :key="carrera" :value="carrera">{{ carrera }}</option>
              </select>
            </div>
          </div>

          <div class="row mb-4" v-if="nuevoAlumno.carrera">
            <div class="col-md-6">
              <h5 class="h6 fw-bold text-primary">Materias Agregadas:</h5>
              <div class="card p-3 bg-light border-0">
                <div v-for="materia in materiasCarrera[nuevoAlumno.carrera].materias" :key="materia" class="form-check">
                  <input class="form-check-input" type="checkbox" v-model="nuevoAlumno.materias" :value="materia" :id="materia">
                  <label class="form-check-label" :for="materia">{{ materia }}</label>
                </div>
              </div>
            </div>

            <div class="col-md-6">
              <h5 class="h6 fw-bold text-primary">Materias de Especialidad:</h5>
              <div class="card p-3 bg-light border-0">
                <div v-for="esp in materiasCarrera[nuevoAlumno.carrera].especialidades" :key="esp" class="form-check">
                  <input class="form-check-input" type="radio" v-model="nuevoAlumno.especialidad" :value="esp" :id="esp">
                  <label class="form-check-label" :for="esp">{{ esp }}</label>
                </div>
              </div>
            </div>
          </div>

          <div class="mb-4">
            <h5 class="h6 fw-bold text-secondary border-bottom pb-2">Actividades Extraescolares:</h5>
            <div class="d-flex flex-wrap gap-3 p-2">
              <div v-for="actividad in actividadesExtraescolares" :key="actividad" class="form-check">
                <input class="form-check-input" type="checkbox" :id="actividad" :value="actividad" v-model="nuevoAlumno.extraescolares" />
                <label class="form-check-label" :for="actividad">{{ actividad }}</label>
              </div>
            </div>
          </div>

          <div class="d-grid shadow-sm">
            <button type="submit" class="btn btn-success btn-lg fw-bold">
               GUARDAR REGISTRO
            </button>
          </div>
        </form>
      </div>
    </div>

    <div class="mt-5">
      <div class="d-flex justify-content-between align-items-center mb-3">
        <h3 class="h4 text-dark mb-0">Listado de Alumnos Registrados</h3>
        <span class="badge bg-secondary">{{ listaAlumnos.length }} alumnos</span>
      </div>
      
      <div class="table-responsive shadow-sm rounded">
        <table class="table table-hover align-middle mb-0" v-if="listaAlumnos.length > 0">
    <thead class="table-dark">
      <tr>
        <th>NC</th>
        <th>Nombre</th>
        <th>Carrera</th>
        <th>Materias</th>
        <th>Especialidad</th>
        <th>Extraescolares</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(al, index) in listaAlumnos" :key="index">
        <td class="fw-bold text-primary">{{ al.numControl }}</td>
        <td>{{ al.nombre }}</td>
        <td><span class="badge bg-info text-dark">{{ al.carrera }}</span></td>
        <td><small class="text-muted">{{ al.materias.join(', ') }}</small></td>
        <td>{{ al.especialidad }}</td>
        <td>
          <small>
            {{ Array.isArray(al.extraescolares) ? al.extraescolares.join(', ') : al.extraescolares }}
          </small>
        </td>
      </tr>
    </tbody>
  </table>
        
        <div v-else class="alert alert-info text-center py-4 mb-0">
           Sin alumnos registrados
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container { max-width: 800px; margin: auto; padding: 20px; }
.formulario { display: flex; flex-direction: column; gap: 15px; background: #f9f9f9; padding: 20px; border-radius: 8px; }
section { display: flex; flex-direction: column; gap: 5px; }
table { width: 100%; border-collapse: collapse; margin-top: 20px; }
th, td { border: 1px solid #ddd; padding: 8px; text-align: left; }
th { background-color: #42b983; color: white; }
</style>