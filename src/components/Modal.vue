<template>
  <div v-if="isVisible" class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50">
    <div class="bg-white p-6 rounded-lg shadow-lg w-full max-w-lg">
      <h2 class="text-xl font-semibold mb-4">{{ mode === 'add' ? 'Add' : mode === 'edit' ? 'Edit' : 'Show' }} Student</h2>
      
      <div v-if="mode !== 'show' && mode !== 'delete'">
        <label class="block mb-2">First Name</label>
        <input v-model="form.firstName" type="text" class="w-full mb-4 px-3 py-2 border rounded-lg" :readonly="mode === 'show'">
        
        <label class="block mb-2">Last Name</label>
        <input v-model="form.lastName" type="text" class="w-full mb-4 px-3 py-2 border rounded-lg" :readonly="mode === 'show'">
        
        <label class="block mb-2">Score</label>
        <input v-model="form.score" type="number" class="w-full mb-4 px-3 py-2 border rounded-lg" :readonly="mode === 'show'">
      </div>
      
      <div v-if="mode == 'show'">
        <p><strong>First Name:</strong> {{ form.firstName }}</p>
        <p><strong>Last Name:</strong> {{ form.lastName }}</p>
        <p><strong>Score:</strong> {{ form.score }}</p>
      </div>

      <div v-if="mode == 'delete'">
        <p>Are you sure you want to delete the student <strong>{{ form.firstName }} {{ form.lastName }}</strong>?</p>
      </div>

      <div class="mt-6 flex justify-end">
        <button @click="close" class="py-2 px-4 mr-2 bg-gray-500 hover:bg-gray-400 text-white rounded-lg">Close</button>
        <button v-if="mode === 'add'" @click="save" class="py-2 px-4 bg-blue-500 hover:bg-blue-400 text-white rounded-lg">Add</button>
        <button v-if="mode === 'edit'" @click="save" class="py-2 px-4 bg-blue-500 hover:bg-blue-400 text-white rounded-lg">Save</button>
        <button v-if="mode === 'delete'" @click="confirmDelete" class="py-2 px-4 bg-red-500 hover:bg-red-400 text-white rounded-lg">Delete</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    isVisible: Boolean,
    mode: String,
    student: Object
  },
  data() {
    return {
      form: {
        id: null,
        firstName: '',
        lastName: '',
        score: null
      }
    }
  },
  watch: {
    student: {
      immediate: true,
      handler(student) {
        if (student) {
          this.form = { ...student };
        }
      }
    }
  },
  methods: {
    close() {
      this.$emit('close');
    },
    save() {
      this.$emit('save', { ...this.form });
      this.close();
    },
    confirmDelete() {
      this.$emit('confirmDelete', { ...this.form });
      this.close();
    }
  }
}
</script>

<style scoped>
</style>
