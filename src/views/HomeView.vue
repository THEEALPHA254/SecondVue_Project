<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import AddMembers from '@/components/AddMembers.vue';
import { useRouter } from 'vue-router';

let all_members = ref([]);
const addmembersref = ref(null);
const showSuccessAlert = ref(false);
const router = useRouter();

const api = axios.create({
  baseURL: 'http://localhost:8000/api/',
  headers: {
    'Content-Type': 'application/json'
  }
});

async function fetchMembers() {
  try {
    const response = await api.get('members/');
    all_members.value = response.data;
  } catch (error) {
    console.error('Error fetching members:', error);
  }
}
async function addMember(member) {
  try {
    await api.post('members/', member);
    showSuccessAlert.value = true;
    setTimeout(() => showSuccessAlert.value = false, 3000); // Hide alert after 3 seconds
    fetchMembers();
  } catch (error) {
    console.error('Error adding member:', error);
  }
}

async function updateMember(id, updatedMember) {
  try {
    await api.put(`members/${id}/`, updatedMember);
    showSuccessAlert.value = true;
    setTimeout(() => showSuccessAlert.value = false, 3000); // Hide alert after 3 seconds
    fetchMembers();
  } catch (error) {
    console.error('Error updating member:', error);
  }
}




onMounted(()=>{
  fetchMembers()
})
</script>


<template>
  <main>
    <h1>REGISTRATION FORM</h1>
    <div class="row mt-4">
      <div class="col-12 col-md-6 offset-md-3">
        <div class="card">
          <div class="card-body">
            <!-- Success alert -->
            <div v-if="showSuccessAlert" class="alert alert-success text-center">
              Operation successful!
            </div>
            <!-- No members alert -->
            <div v-if="all_members.length === 0" class="alert alert-warning text-center">
              No members registered.
            </div>
            <AddMembers ref="addmembersref" @addMember="addMember" @updateMember="updateMember" />
          </div>
        </div>
      </div>
    </div>
  </main>
</template>