<template>
<nav class="navbar border container">
      <div class="container-fluid">
        <div class="dropdown" style="margin-left: 100px">
          <button class="btn btn-secondary dropdown-toggle bg-primary" type="button" data-bs-toggle="dropdown" aria-expanded="false">
            {{genderFilter}}
          </button>
          <ul class="dropdown-menu">
            <li><a @click="allGender" class="dropdown-item">All</a></li>
            <li><a @click="femaleGender" class="dropdown-item">Female</a></li>
            <li><a @click="maleGender" class="dropdown-item">Male</a></li>
          </ul>
        </div>
        <div class="dropdown" style="margin-left: 700px">
          <button class="btn btn-secondary dropdown-toggle bg-primary" type="button" data-bs-toggle="dropdown" aria-expanded="false">
            {{numofUsers}}
          </button>
          <ul class="dropdown-menu">
            <li><a @click="twentyUsers" class="dropdown-item">20</a></li>
            <li><a @click="tenUsers" class="dropdown-item">10</a></li>
            <li><a @click="fiveUsers" class="dropdown-item">5</a></li>
          </ul>
        </div>
        <nav aria-label="Page navigation example" style="margin-right: 100px;">
  <ul class="pagination">
    <li class="page-item">
      <p @click="firstPage" class="page-link" aria-label="Previous">
        <span aria-hidden="true">&laquo;</span>
      </p>
    </li>
    <li class="page-item"><p @click="prevPage" class="page-link active" id="page1">{{currentpage}}</p></li>
    <li class="page-item"><p @click="currPage" class="page-link" id="page2">{{currentpage+1}}</p></li>
    <li class="page-item"><p @click="curr2Page" class="page-link" id="page3">{{currentpage+2}}</p></li>
    <li class="page-item"><p @click="nextPage" class="page-link" id="page4">{{currentpage+3}}</p></li>
    <li class="page-item">
      <p @click="lastPage" class="page-link" aria-label="Next">
        <span aria-hidden="true">&raquo;</span>
      </p>
    </li>
  </ul>
</nav>
</div>
</nav>
<Users 
:filterByGender="filterByGender"
:userpage="userpage"
:showModal="showModal"
@toggles-modal="toggleModal"/>
</template>

<script lang="ts">

import { computed, ComputedRef, defineComponent, ref } from 'vue';
import {RootObject, Result} from '@/types/Results'
import Users from '@/components/Users.vue';

export default defineComponent({

    components: {Users},
    
    setup() {

      const results = ref<RootObject>()
      const userpage = ref<number[]>([0, 20])
      const currentpage = ref<number>(1)
      const page = ref<number>(1)
      const numofUsers = ref<number>(20)
      const totalusers = ref<number>(100)
      const genderFilter = ref<string>("All")
      const showModal = ref<boolean[]>([false])

      const load = async () => {
        try 
        {
          let data = await fetch('https://randomuser.me/api/?results='+totalusers.value)
          if(!data.ok)
          {
            throw Error('no data available')
          }
          results.value = await data.json()
          for(let a = 0; a < totalusers.value; a++)
          {
            showModal.value?.push(false)
          }
        }
        catch(err: any)
        {
          console.log(err.message)
        }
      }

      load()

      const removeActive = () =>
      {
        document.getElementById("page1")?.classList.remove("active")
        document.getElementById("page2")?.classList.remove("active")
        document.getElementById("page3")?.classList.remove("active")
        document.getElementById("page4")?.classList.remove("active")
      }

      const removeDisabled = () =>
      {
        document.getElementById("page1")?.classList.remove("disabled")
        document.getElementById("page2")?.classList.remove("disabled")
        document.getElementById("page3")?.classList.remove("disabled")
        document.getElementById("page4")?.classList.remove("disabled")
      }

      const addDisabled = () =>
      {
        currentpage.value = 1
         document.getElementById("page4")?.classList.add("disabled")
         if(totalusers.value/numofUsers.value <= 2)
         {
          document.getElementById("page3")?.classList.add("disabled")
         }
         if(totalusers.value/numofUsers.value <= 1)
         {
          document.getElementById("page2")?.classList.add("disabled")
         }  
      }

      const filterTotalUsers = () =>
      {
        if(filterByGender.value?.length != undefined)
        {
          totalusers.value = filterByGender.value.length
          if(filterByGender.value.length / numofUsers.value <= 3)
          {
            if(filterByGender.value.length / numofUsers.value < currentpage.value+(page.value-1))
            {
              lastPage()
            } 
            else
            {
              adjustPage()
            }
          }
          else
          {
            if((filterByGender.value.length / numofUsers.value <= currentpage.value+2 && page.value >= 3) || (filterByGender.value.length / numofUsers.value <= currentpage.value+1 && page.value <= 2))
            {
              lastPage()
            }
            else
            {
              if(filterByGender.value.length / numofUsers.value <= currentpage.value+2)
              {
                currentpage.value = Math.ceil((totalusers.value/numofUsers.value) - 3)
                page.value = 3
              }
              adjustPage()
            }
          }
        }
      }

      const prevPage = () =>
      {
        removeActive()
        if(currentpage.value != 1)
        {
          currentpage.value--
          userpage.value[0] = (currentpage.value) * numofUsers.value
          userpage.value[1] = userpage.value[0] + numofUsers.value
          document.getElementById("page2")?.classList.add("active")
          page.value = 2
        }
        else
        {
          userpage.value[0] = (currentpage.value-1) * numofUsers.value
          userpage.value[1] = userpage.value[0] + numofUsers.value
          document.getElementById("page1")?.classList.add("active")
          page.value = 1
        }
      }

      const currPage = () =>
      {
        removeActive()
        userpage.value[0] = (currentpage.value) * numofUsers.value
        userpage.value[1] = userpage.value[0] + numofUsers.value
        document.getElementById("page2")?.classList.add("active")
        page.value = 2
      }

      const curr2Page = () =>
      {
        removeActive()
        userpage.value[0] = (currentpage.value+1) * numofUsers.value
        userpage.value[1] = userpage.value[0] + numofUsers.value
        document.getElementById("page3")?.classList.add("active")
        page.value = 3
      }

      const nextPage = () =>
      {
        removeActive()
        if(currentpage.value+3 < (totalusers.value / numofUsers.value))
        {
          currentpage.value++
          userpage.value[0] = (currentpage.value+1) * numofUsers.value
          userpage.value[1] = userpage.value[0] + numofUsers.value
          document.getElementById("page3")?.classList.add("active")
          page.value = 3
        }
        else
        {
          userpage.value[0] = (currentpage.value+2) * numofUsers.value
          userpage.value[1] = userpage.value[0] + numofUsers.value
          document.getElementById("page4")?.classList.add("active")
          page.value = 4
        }
      }

      const firstPage = () =>
      {
        currentpage.value = 1
        prevPage()
      }

       const lastPage = () =>
      {
        removeDisabled()
        if(totalusers.value/numofUsers.value <= 3)
        {
         addDisabled()
         page.value = Math.ceil(totalusers.value/numofUsers.value)
         adjustPage()
        }
        else
        {
          currentpage.value = Math.ceil((totalusers.value/numofUsers.value) - 3)
          nextPage()
        }
      }

      const twentyUsers = () =>
      {
        numofUsers.value = 20
        if(currentpage.value > (totalusers.value/numofUsers.value) - 3)
        {
          lastPage()
        }
        else{
          adjustPage()
        }
      }

      const tenUsers = () =>
      {
        numofUsers.value = 10
        if(currentpage.value > (totalusers.value/numofUsers.value) - 3)
        {
          lastPage()
        }
        else{
          adjustPage()
        }
      }

      const fiveUsers = () =>
      {
        numofUsers.value = 5
        if(currentpage.value > (totalusers.value/numofUsers.value) - 3)
        {
          lastPage()
        }
        else{
          adjustPage()
        }
      }

      const adjustPage = () =>
      {
        removeDisabled()
        if(totalusers.value/numofUsers.value <= 3)
        {
              addDisabled()
        }
        if(page.value == 1)
        {
          prevPage()
        }
        else if(page.value == 2)
        {
          currPage()
        }
        else if(page.value == 3)
        {
          curr2Page()
        }
        else
        {
          nextPage()
        }
      }

      const filterByGender: ComputedRef<Result[] | undefined> = computed(() =>
      {
        if(genderFilter.value == "All")
        {
          return results.value?.results
        }
        else
        {
          return results.value?.results.filter((user) => user.gender == genderFilter.value.toLowerCase())
        }
      })

      const allGender = () =>
      {
        for(let a = 0; a < totalusers.value; a++)
        {
          showModal.value?.push(false)
        }
        genderFilter.value = "All"
        filterTotalUsers()
      }

      const femaleGender = () =>
      {
        for(let a = 0; a < totalusers.value; a++)
        {
            showModal.value?.push(false)
        }
        genderFilter.value = "Female"
        filterTotalUsers()
      }

      const maleGender = () =>
      {
        for(let a = 0; a < totalusers.value; a++)
        {
            showModal.value?.push(false)
        }
        genderFilter.value = "Male"
        filterTotalUsers()
      }

       const toggleModal = (index: number) =>
        {
          showModal.value[index] = !showModal.value[index]
        }
      
      return {userpage, currentpage, prevPage, currPage, curr2Page, 
      nextPage, firstPage, lastPage, numofUsers, twentyUsers, tenUsers, 
      fiveUsers, filterByGender, allGender, femaleGender, maleGender, genderFilter, showModal, toggleModal}
    },
})
</script>
