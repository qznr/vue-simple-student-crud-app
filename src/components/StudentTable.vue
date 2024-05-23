<template>
  <div class="mt-6 shadow-sm border rounded-lg overflow-auto">
    <table class="w-full table-auto text-sm text-left">
      <thead class="text-gray-600 font-medium border-b">
        <tr>
          <th class="py-3 px-6">ID</th>
          <th class="py-3 px-6">First Name</th>
          <th class="py-3 px-6">Last Name</th>
          <th class="py-3 px-6">Score</th>
          <th class="py-3 px-6 text-right">Actions</th>
        </tr>
      </thead>
      <tbody class="text-gray-600 divide-y">
        <tr v-for="student in students" :key="student.id">
          <td class="py-3 px-6">{{ student.id }}</td>
          <td class="py-3 px-6">{{ student.firstName }}</td>
          <td class="py-3 px-6">{{ student.lastName }}</td>
          <td class="py-3 px-6">{{ student.score }}</td>
          <td class="text-right px-6">
            <button @click="$emit('showStudent', student)" class="py-2 px-3 my-2 bg-cyan-500 hover:bg-cyan-300 rounded-md text-white font-medium">Show</button>
            <button @click="$emit('editStudent', student)" class="py-2 px-3 my-2 ml-2 bg-purple-500 hover:bg-purple-300 rounded-md text-white font-medium">Edit</button>
            <button @click="$emit('deleteStudent', student)" class="py-2 px-3 my-2 ml-2 bg-red-500 hover:bg-red-300 rounded-md text-white font-medium">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  props: {
    students: {
      type: Array,
      required: true
    }
  },
}
</script>
