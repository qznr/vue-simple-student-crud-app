<template>
  <div class="max-w-screen-xl mx-auto px-4 md:px-8 mt-16">
    <div class="mt-3 md:mt-0">
      <button @click="openAddModal" class="inline-block px-4 py-2 text-white duration-150 font-medium bg-indigo-600 rounded-lg hover:bg-indigo-500 active:bg-indigo-700 md:text-sm">
        Add Mahasiswa
      </button>
    </div>
    <StudentTable :students="students" @showStudent="openShowModal" @editStudent="openEditModal" @deleteStudent="openDeleteModal" />
    <Modal v-if="isModalVisible" :isVisible="isModalVisible" :mode="modalMode" :student="selectedStudent" @close="closeModal" @save="saveStudent" @confirmDelete="confirmDelete" />  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue';
import axios from 'axios';
import StudentTable from './components/StudentTable.vue';
import Modal from './components/Modal.vue';

// const students = ref([
//   { id: 1, firstName: 'Moch. Gustav', lastName: 'Ali Rafsandjani', score: 100 },
//   { id: 2, firstName: 'Emilia', lastName: 'Syah Putri', score: 100 },
// ]);

const students = ref([]);

onMounted(async () => {
  try {
    const response = await axios.get('http://localhost:3000/students');
    students.value = response.data;
  } catch (error) {
    console.error('Error fetching students:', error);
  }
});

const isModalVisible = ref(false);
const modalMode = ref('');
const selectedStudent = ref({ id: null, firstName: '', lastName: '', score: null });

function openAddModal() {
  modalMode.value = 'add';
  selectedStudent.value = { id: null, firstName: '', lastName: '', score: null };
  isModalVisible.value = true;
}

function openEditModal(student) {
  modalMode.value = 'edit';
  selectedStudent.value = { ...student };
  isModalVisible.value = true;
}

function openShowModal(student) {
  modalMode.value = 'show';
  selectedStudent.value = { ...student };
  isModalVisible.value = true;
}

function openDeleteModal(student) {
  modalMode.value = 'delete';
  selectedStudent.value = { ...student };
  isModalVisible.value = true;
}

async function deleteStudent(student) {
  try {
    console.log(student.id)
    await axios.delete(`http://localhost:3000/students/${student.id}`);
    students.value = students.value.filter(s => s.id !== student.id);
  } catch (error) {
    console.error('Error deleting student:', error);
  }
}

function closeModal() {
  isModalVisible.value = false;
  selectedStudent.value = { id: null, firstName: '', lastName: '', score: null };
}

function confirmDelete() {
  console.log(selectedStudent.value)
  deleteStudent(selectedStudent.value);
  console.log(selectedStudent.value)
  closeModal();
}

async function saveStudent(student) {
  try {
    if (modalMode.value === 'add') {
      // Menentukan ID baru dengan menambahkan 1 dari ID tertinggi yang sudah ada
      const maxId = students.value.length ? Math.max(...students.value.map(s => s.id)) : 0;
      student.id = (maxId + 1).toString();
      
      const response = await axios.post('http://localhost:3000/students', student);
      students.value.push(response.data);
    } else {
      const response = await axios.put(`http://localhost:3000/students/${student.id}`, student);
      const index = students.value.findIndex(s => s.id === student.id);
      if (index !== -1) {
        students.value.splice(index, 1, response.data);
      }
    }
    closeModal();
  } catch (error) {
    console.error('Error saving student:', error);
  }
}

</script>
