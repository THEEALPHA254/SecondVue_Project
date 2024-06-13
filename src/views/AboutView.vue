<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import MembersList from '@/components/MembersList.vue';
import { useRouter } from 'vue-router';

let all_members = ref([]);
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

async function deleteMember(id) {
  try {
    await api.delete(member/${id}/);
    all_members.value = all_members.value.filter(member => member.id !== id);
  } catch (error) {
    console.error('Error deleting member:', error);
  }
}

function editMember(id) {
  const member = all_members.value.find(member => member.id === id);
  router.push({ name: 'Members', query: { edit: member } });
}
onMounted(()=>{
  fetchMembers()
})

</script>

<template>
  <main>
  <h1 >REGISTERED MEMBERS</h1>
    <div class="row mt-4">
      <div class="col-12 col-md-6 offset-md-3">
        <div class="card">
          <div class="card-body">
              <!--This div container contains the list of memebrs registered-->
              <div>
                <MembersList :all_members="all_members" @deleteMember="deleteMember" @editMember="editMember" />
              </div>
              
              
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style>

</style>