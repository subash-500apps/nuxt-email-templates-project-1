<template>
    <div v-for="(template, index) in templates" :key="index" >
         <div >
                  <span>
              <MapIcon v-if="template.type"  class="h-5 w-5 font-[500] text-gray-600 mr-2 sm:mt-[20px]"/>
           </span>
            <!-- End of icon -->
            <div class="ml-[50px] mt-[-20px]">
                <p class="text-b">
                    {{ template.name }}
                </p>
                <p class="text-gray-600">{{ template.subject }}</p>
                <p class="text-sm text-gray-600 mb-[90px]">{{ (template.body) }}</p>
            </div>
        </div>
        <span>
            <TrashIcon class="h-5 w-5 text-gray-600 ml-[15rem] mt-[-120px]" @click="() => emit('deletetemplate', template.uid)" />
        </span>
    </div>
</template>

<script setup lang="ts">
import { EnvelopeIcon, CodeBracketIcon, DocumentIcon,TrashIcon } from '@heroicons/vue/24/outline'
import { MapIcon } from '@heroicons/vue/24/outline'

interface Templates{
    templates:Array<[{}]>
}
// Define props
const props = withDefaults(defineProps<Templates>(), {
     templates:()=>{
        return []
    }
})

// Define emits
const emit = defineEmits(['deletetemplate'])


</script>
<style scoped></style>