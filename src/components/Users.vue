<template>
<div class="border container bg-light">
  <div v-for="(result, index) in filterByGender" :key="index" style="display: inline-block">
    <div class="card" style="width: 13rem; height: 32rem; margin: 10px; margin-top: 40px;" v-if="index >= userpage[0] && index < userpage[1]">
      <img :src="result.picture.large" class="card-img-top" alt="...">
      <div class="card-body">
        <h5 class="card-title" style="font-size: 14px; font-weight: bold; margin-top: 5px">{{result.name.title}} {{result.name.first}} {{result.name.last}}</h5>
        <p class="card-text" style="font-size: 12px; text-align: left;"><b>Gender:</b> {{result.gender}}<br><b>Age:</b> {{result.dob.age}}<br><b>Birthdate:</b> {{new Date(result.dob.date).toDateString()}}<br><b>Email:</b> {{result.email}}<br><b>Location:</b> {{result.location.city}}, {{result.location.country}}<br><b>Phone:</b> {{result.phone}}</p>
      </div>
      <button type="button" class="btn btn-primary" @click="toggleModal(index)" style="font-size: 12px; width: 100px; margin-left: 50px; margin-bottom: 20px">More</button>
      <Modal
      v-if="showModal[index]" 
      :filterByGender="result" 
      :index="index"
      @toggle-modal="toggleModal"/>
    </div>
  </div>
<br><br>
</div>
</template>
<script lang="ts">
import { defineComponent, PropType, ref } from 'vue'
import {Result} from '@/types/Results'
import Modal from './Modal.vue'

export default defineComponent({
    emits: ['toggles-modal'],
    components: {Modal},
    props: {
        filterByGender: {
            required: true,
            type: Array as PropType<Result[] | undefined>
        },
        userpage: {
            required: true,
            type: Array as PropType<number[]>
        },
        showModal: {
            required: true,
            type: Array as PropType<boolean[]>
        }
    },
    setup(props, {emit}) {
        const toggleModal = (index: number) =>
        {
          emit('toggles-modal', index)
        }
        return {toggleModal}
    },
})
</script>
