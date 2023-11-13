<template>
  <div class="q-pa-md">
    <q-table
      :rows="rows"
      :columns="columns"
      row-key="qa_id"
      class="q-pt-lg"
      title="待審核問題"
      v-model:pagination="pagination"
      hide-pagination
      style="background: rgb(255, 254, 254)"
    >
      <template v-slot:top-right>
        <Search />
        <OrderSelect />
      </template>
      <template v-slot:body-cell-actions="props">
        <q-td :props="props">
          <q-btn
            dense
            round
            flat
            icon="edit"
            class="q-mx-md"
            @click="editPop(props.row)"
          ></q-btn>
          <q-btn
            dense
            round
            flat
            icon="delete"
            class="q-mx-md"
            @click="deleteSubmit"
          ></q-btn>
        </q-td>
      </template>
      <template #bottom>
        <div style="right: 30px; position: absolute">
          <q-pagination
            v-model="pagination.page"
            :max="pagesNumber"
            max-pages="6"
            boundary-numbers
            direction-links
            active-color="accent"
            unelevated
            color="secondary"
            text-color="black"
            active-text-color="white"
          />
        </div>
      </template>
    </q-table>
  </div>
  <q-dialog v-model="open">
    <q-card>
      <q-card-section class="row items-center q-pb-none">
        <q-input v-model="selectRow.qa_question" dense type="textarea" autogrow
          ><template v-slot:prepend>
            <span class="text-subtitle1">Q:&nbsp;</span>
          </template>
        </q-input>
        <q-space />
        <q-btn icon="close" flat round dense v-close-popup />
      </q-card-section>

      <q-card-section>
        <q-input v-model="selectRow.qa_answer" dense type="textarea" autogrow
          ><template v-slot:prepend>
            <span class="text-subtitle1">A:&nbsp;</span>
          </template>
        </q-input>
      </q-card-section>
      <q-card-actions>
        <q-select
          filled
          v-model="selectRow.office_name"
          :options="options"
          class="q-mr-sm"
          dense
          options-dense
        >
          <template v-slot:prepend>
            <span class="text-subtitle2">指派單位:</span>
          </template>
        </q-select>
        <q-btn flat label="取消" color="primary" v-close-popup />
        <q-btn
          flat
          label="修改"
          color="primary"
          v-close-popup
          @click="editSubmit"
        />
      </q-card-actions>
    </q-card>
  </q-dialog>
  {{ selectRow }}
</template>

<script setup lang="ts">
import { ref, computed, watch, reactive } from 'vue';
import Search from './Components/Table/SearchTable.vue';
import OrderSelect from './Components/Table/OrderTable.vue';
import { columns } from './Components/Table/Columns';
import Swal from 'sweetalert2';

const update = ref(0);
const pagination = ref({
  sortBy: 'desc',
  descending: false,
  page: 1,
  rowsPerPage: 3,
  // rowsNumber: xx if getting data from a server
});

const pagesNumber = computed(() =>
  Math.ceil(rows.length / pagination.value.rowsPerPage)
);

const open = ref(false);
const selectRow = ref(null);
const editPop = (data: any) => {
  open.value = true;
  selectRow.value = data;
};
watch(open, () => {
  if (open.value) return;
  selectRow.value = 'nothing';
});

const options = computed(() => {
  return offices
    .filter((x) => x.office_name !== selectRow.value.office_name)
    .map((x) => x.office_name);
});
const offices = [
  { office_id: 1, office_name: '資管' },
  { office_id: 2, office_name: '統資' },
  { office_id: 3, office_name: '圖資' },
];
const editSubmit = () => {
  const officeId = offices.filter(
    (x) => x.office_name === selectRow.value.office_name
  )[0].office_id;

  const editData = { ...selectRow.value, office_id: officeId };
  Swal.fire({
    showConfirmButton: false,
    title: '修改成功',
    icon: 'success',
    timer: 1200,
  });
};
const deleteSubmit = () => {
  Swal.fire({
    title: '確定刪除?',
    icon: 'warning',
    showCancelButton: true,
    confirmButtonColor: '#d33',
    cancelButtonColor: '#3085d6',
    confirmButtonText: '確認刪除',
    cancelButtonText: '取消',
  }).then((result) => {
    if (result.isConfirmed) {
      // const del = async () => {
      //   await deleteDoc(doc(db, 'users', id));
      // };
      // del();
      Swal.fire({
        showConfirmButton: false,
        title: '刪除成功',
        icon: 'success',
        timer: 1200,
      });
    }
  });
};
const rows = [
  {
    qa_id: 1,
    qa_question: 'Frozen Yogurt',
    qa_answer: 'answer',
    office_id: 1,
    office_name: '資管',
    user_id: '111',
    qa_asktime: '110/1/2',
  },
  {
    qa_id: 2,
    qa_question: 'Frozen Yogurt',
    qa_answer: 'answer',
    office_id: 1,
    office_name: '資管',
    user_id: '1122',
    qa_asktime: '110/1/23',
  },
  {
    qa_id: 3,
    qa_question: 'Frozen Yogurt',
    qa_answer: 'answer',
    office_id: 1,
    office_name: '資管',
    user_id: '121',
    qa_asktime: '110/3/2',
  },
  {
    qa_id: 4,
    qa_question: 'Frozen Yogurt',
    qa_answer: 'answer',
    office_id: 1,
    office_name: '資管',
    user_id: '121',
    qa_asktime: '110/3/2',
  },
  {
    qa_id: 5,
    qa_question: 'Frozen Yogurt',
    qa_answer: 'answer',
    office_id: 1,
    office_name: '資管',
    user_id: '121',
    qa_asktime: '110/3/2',
  },
  {
    qa_id: 6,
    qa_question: 'Frozen Yogurt',
    qa_answer: 1,
    office_id: '001',
    office_name: '資管',
    user_id: '121',
    qa_asktime: '110/3/2',
  },
  {
    qa_id: 7,
    qa_question: 'Frozen Yogurt',
    qa_answer: 1,
    office_id: '001',
    office_name: '資管',
    user_id: '121',
    qa_asktime: '110/3/223',
  },
];

// :loading="true"
//loading-label="Gimme a sec and I'll fetch ya data!"
//no-data-label="I can't find any data 😞"
</script>
