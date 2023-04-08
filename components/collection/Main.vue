<template>
    <!--Add template-->
      <div class="pb-3 w-[100%]">
          <button
            class="bg-white hover:bg-gray-50 hover:text-gray-800 border focus-visible:outline focus-visible:outline-2 focus-visible:outline-indigo-600 focus-visible:outline-offset-2 font-semibold inline-flex items-center justify-center px-3 py-3 rounded-md shadow-sm text-gray-600 text-sm w-[100%]"
            @click="addTemplate=true"      
            >
            <span>
              <PlusIcon class="h-5 w-5 mr-2" />
            </span>
            Add Template
          </button>
        </div>
        <!--Template List-->
<div class="grid grid-rows-3 grid-flow-col grid-cols-4 border w-[80vw] mx-auto my-5 rounded-lg pr-[4px]"
    >
  <CollectionList :templates="templates" @deletetemplate="deleteItem"></CollectionList>

  <!--Add Template-->
      <CollectionAdd
        v-show="addTemplate"
        @save="save"
        @hide="(value)=>addTemplate=value"
            />
</div>
    
</template>
<script setup lang="ts">
import { PlusIcon } from "@heroicons/vue/24/outline"
const  url= "https://v1-orm-lib.mars.hipso.cc/api/email-templates/?offset=0&limit=100&sort_column=id&sort_direction=desc"
const authHeader = {
  Authorization:
    "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1IjoiYzI4OGRlNzBjYWYyNDUxODk4ZDRhMGM2N2U0ZmVkZDIiLCJkIjoiMTY4MDA4OSIsInIiOiJzYSIsInAiOiJmcmVlIiwiYSI6ImZpbmRlci5pbyIsImwiOiJ1czEiLCJleHAiOjE2ODMyODExOTV9.gl7Z_P_zA3fGoIH9mulaZF4tcA2vvln4x_dremExFIo",
}
let templates=ref([])
const addTemplate=ref(false)
interface Template {
  url: string;
  isActive: string;
  projectId: string;
  shareType: string;
  entity: string;
}
const props = withDefaults(defineProps<Template>(), {

  url: "https://v1-orm-lib.mars.hipso.cc/api/email-templates/",
  isActive: "1",
  projectId: "10",
  shareType: "PRIVATE",
  entity: "Leads",
});
// Get templates data using api call
const { pending, data } = useLazyFetch(
  `${url}`,
  { method: "GET", headers: authHeader }
  );

// Assign templates data after get call
if(data) templates=data
const save = async (template: any) => {
  const staticData = ref({
    project_id: props.projectId,
    name: template.name,
    subject: template.subject,
    body: template.body,
    is_active: props.isActive,
    type: "PLAIN_TEXT",
    share_type: props.shareType,
    category: props.entity,
  });
  // Post call hits when click on save button
  await useLazyFetch(props.url, {
    method: "POST",
    headers: authHeader,
    body: staticData.value,
  });
  templates.value.unshift(staticData.value);
 }
 const deleteItem = (templateUid: string) => {
  const { data: response } = useLazyFetch(`${props.url}${templateUid}`, {
    method: "DELETE",
    headers: authHeader,
  });
  if (response) {
    const index = templates.value.findIndex(
      (template: any) => template.uid === templateUid
    );
    if (index !== -1) {
      templates.value.splice(index, 1);
    }
  }}
</script>