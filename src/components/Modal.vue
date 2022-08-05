<template>
<section class="u-align-center u-black u-clearfix u-container-style u-dialog-block u-opacity u-opacity-70 u-valign-middle-lg u-valign-middle-md u-valign-middle-sm u-valign-middle-xl u-dialog-section-4" id="carousel_7856">
      <div class="u-container-style u-dialog u-shape-rectangle u-white u-dialog-1">
        <div class="u-container-layout u-valign-bottom-lg u-valign-bottom-md u-valign-bottom-xl u-container-layout-1">
          <img alt="" class="u-expanded-width-xs u-image u-image-default u-image-1" data-image-width="608" data-image-height="700" :src="filterByGender?.picture.large">
          <div class="u-align-center u-container-style u-group u-shape-rectangle u-group-1">
            <div class="u-container-layout u-container-layout-2">
              <h3 class="u-align-center u-custom-font u-font-playfair-display u-text u-text-palette-5-dark-2 u-text-1">{{filterByGender?.name.title}} {{filterByGender?.name.first}} {{filterByGender?.name.last}}
              </h3>
              <p class="u-align-center u-text u-text-default u-text-2" style="text-align: left;"><b>Gender:</b> {{filterByGender?.gender}}<br><b>Age:</b> {{filterByGender?.dob.age}}<br><b>Birthdate:</b> {{filterByGender?.dob.date}}<br><b>Location:</b> {{filterByGender?.location.street.number}} {{filterByGender?.location.street.name}}, {{filterByGender?.location.city}}, {{filterByGender?.location.state}}, {{filterByGender?.location.postcode}}, {{filterByGender?.location.country}}<br><b>Email:</b> {{filterByGender?.email}}<br><b>UserName:</b> {{filterByGender?.login.username}}<br><b>Phone:</b> {{filterByGender?.phone}}<br><b>Cellphone:</b> {{filterByGender?.cell}}
              </p>
            </div>
          </div>
        </div>
        <button class="u-dialog-close-button u-icon u-text-grey-50 u-icon-1" @click="toggleModal(index)">
        <svg class="u-svg-link" preserveAspectRatio="xMidYMin slice" viewBox="0 0 409.806 409.806" style=""><use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#svg-4e2d"></use></svg>
        <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" xml:space="preserve" class="u-svg-content" viewBox="0 0 409.806 409.806" x="0px" y="0px" id="svg-4e2d" style="enable-background:new 0 0 409.806 409.806;"><g><g><path d="M228.929,205.01L404.596,29.343c6.78-6.548,6.968-17.352,0.42-24.132c-6.548-6.78-17.352-6.968-24.132-0.42    c-0.142,0.137-0.282,0.277-0.42,0.42L204.796,180.878L29.129,5.21c-6.78-6.548-17.584-6.36-24.132,0.42    c-6.388,6.614-6.388,17.099,0,23.713L180.664,205.01L4.997,380.677c-6.663,6.664-6.663,17.468,0,24.132    c6.664,6.662,17.468,6.662,24.132,0l175.667-175.667l175.667,175.667c6.78,6.548,17.584,6.36,24.132-0.42    c6.387-6.614,6.387-17.099,0-23.712L228.929,205.01z"></path>
        </g>
        </g></svg>
      </button>
    </div>
</section>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue'
import { Result } from '@/types/Results'

export default defineComponent({
    emits: ['toggle-modal'],
    props: {
        filterByGender: {
            required: true,
            type: Object as PropType<Result | undefined>
        },
        index: {
            required: true,
            type: Number
        }
    },
    setup(props, {emit}) {
        const toggleModal = (index: number) =>
        {
          emit('toggle-modal', index)
        }
        return {toggleModal}
    },
})
</script>
