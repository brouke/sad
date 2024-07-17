<template>
  <div class="app">
    <div class="app__head">
      <div class="app__head__info">
        <div class="app__head_logo">
        <img src="/logo.png"/>
      </div>
      <div class="app__head-select">
        <Input 
        placeholder="Поиск по имени"
        inputclass="search"
        v-model="searchName"
        />
        <Input 
        placeholder="Поиск по компании"
        inputclass="search"
        v-model="searchCompany"
        />
        <Button @click="ModalOpen = true"
        placeholder="Добавить"
        class="btn__add-button"
        />
      </div>
      </div>
      <div class="app__head__quantity">
        <div>
          <p class="app__head__staff">Посетители</p>
        </div>
        <div class="app__head__counter">
          <p class="app__head__present">{{present}}</p>/<p class="app__head__absent">{{ absent }}</p>
        </div>
      </div>
    </div>
    <div class="app__body">
      <ModalsAddStaff v-if="ModalOpen"
      @close="ModalOpen = false"  
      @update ="refresh()"
    />
    <ModalsEditStaff 
      v-if="EditModalOpen"
      @close="EditModalOpen = false"  
      @update ="refresh()"
      :item="edits"
    />
    <table>
  <thead>
    <tr>
      <th>Номер</th>
      <th>ФИО</th>
      <th>Компания</th>
      <th>Группа</th>
      <th class="table__th">Присутствие</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="item in filteredItems" @click="edit(item)">
      <td>{{ item.id }}</td>
      <td>{{ item.name }}</td>
      <td>{{ item.company }}</td>
      <td>{{ item.group }}</td>
      <td class="table__td">
        <div class="absent" v-if="item.qauntity"></div>
        <div class="present" v-if="!item.qauntity"></div>
      </td>
    </tr>
  </tbody>
  </table>
    </div>
  <div class="app__filter">
    <p>Фильтровать по:</p>
      <Button 
        @click="filterItems('absent')"
        placeholder="Отсутствующим"
        class="btn__filter-button"
      />
      <Button 
        @click="filterItems('present')"
        placeholder="Присутствующим"
        class="btn__filter-button"
      />
      <Button 
        @click="filterItems('all')"
        placeholder="Без фильтра"
        class="btn__filter-button"
      />
    </div>
  </div>
</template>
<style>
@import '@/assets/scss/index.scss';
</style>
<script setup>
const searchName = ref('');
const searchCompany = ref('');
const items = ref([]);
const present = ref(null);
const absent = ref(null);
const EditModalOpen = ref(false);
const ModalOpen = ref(false);
const edits = ref([])

const filteredItems = computed(() => {
  return items.value.filter(item =>
    item.name.toLowerCase().includes(searchName.value.toLowerCase()) &&
    item.company.toLowerCase().includes(searchCompany.value.toLowerCase())
  );
});
async function edit(item){
  edits.value = item;
  EditModalOpen.value = true;
}
async function refresh(){
  items.value = JSON.parse(localStorage.getItem('items'));
  present.value = items.value.filter(item => item.qauntity).length;
  absent.value = items.value.filter(item => !item.qauntity).length;
}

onMounted(() => {
  if (localStorage.getItem('items')) {
    items.value = JSON.parse(localStorage.getItem('items'));
    present.value = items.value.filter(item => item.qauntity).length;
    absent.value = items.value.filter(item => !item.qauntity).length;
  }
});

  onMounted(() => {
    document.addEventListener('keydown', function(e) {
    if (e.key == "Escape") {
        ModalOpen.value = false;
    }
});
  });

  const filterItems = (filter) => {
    const tilt = JSON.parse(localStorage.getItem('items'));
  if (filter === 'all') {
    items.value = tilt;
  } else if (filter === 'present') {
    items.value = tilt.filter(item => item.qauntity);
  } else if (filter === 'absent') {
    items.value = tilt.filter(item => !item.qauntity);
  }
};
</script>
<style lang="scss">

.app{
    display: grid;
    padding: 24px 50px 10px 50px;
    grid-template-rows: 0.1fr 10fr 0.1fr;
    @media screen and (width < 1820px){
      grid-template-rows: 0.1fr 8fr 0.1fr;
    }
    @media screen and (width < 1620px){
      grid-template-rows: 0.1fr 4.5fr 0.1fr;
    }
    @media screen and (width < 1340px){
      grid-template-rows: 0.1fr 2.3fr 0.1fr;
    }
    @media screen and (width < 750px){
      padding: 0px 10px;
    }
  &__head{
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 90px;
    @media screen and (width < 1620px){
      height: 150px;
    }
    @media screen and (width < 1340px){
      height: 250px;
    }
    @media screen and (width < 940px){
      height: 400px;
      justify-content: space-evenly;
    }
    @media screen and (width < 750px){
      justify-content: center;
        gap: 40px;
        align-items: center;
    }
    &__info{
      display: flex;
      gap: 40px;
      @media screen and (width < 940px){
      display: grid;
      justify-items: center;
    }
    }
    
    &__quantity{
      display: grid;
      justify-items: end;
    }
    &__counter{
      display: flex;
      font-family: OpenSans;
      font-size: 30px;
      font-weight: 700;
      line-height: 40.85px;
      color: #4E3000;
    }
    &-select{
      display: flex;
      gap: 40px;
      align-items: center;
      @media screen and (width < 1620px){
      display: grid;
      grid-template-columns: 1fr 1fr;
    }
    @media screen and (width < 1340px){
      display: grid;
      grid-template-columns: 1fr;
      justify-items: center;
    }
    }
    &__present{
      font-family: OpenSans;
      font-size: 30px;
      font-weight: 700;
      line-height: 40.85px;
      margin: 0px;
      padding: 0px 12px;
      color: #80BB00;
    }
    &__absent{
      font-family: OpenSans;
      font-size: 30px;
      font-weight: 700;
      line-height: 40.85px;
      margin: 0px;
      padding: 0px 12px;
      color: #EC5937;
    }
    &__staff{
      font-family: OpenSans;
      font-size: 30px;
      font-weight: 700;
      line-height: 40.85px;
      margin: 0px;
      color: #4E3000;
    }
  }
  &__body{
    height: 100%;
    overflow-y: auto;
  }
  &__filter{
    display: flex;
    align-items: center;
    height: 53px;
    p{
      font-family: Open Sans;
      font-size: 30px;
      font-weight: 700;
      line-height: 40.85px;
      text-align: left;
      color: #4E3000;
    }
  }
}
table{
  width: 100%;
  text-align: center;
  border-collapse: separate;
    border-spacing: 0 1em;
}
tr:hover{
  background-color: rgba(108, 110, 110, 0.302);
  cursor: pointer;
}
th{
  border-bottom: 4px solid #E9E9E9;
  padding: 0px;
  font-family: OpenSans;
  font-size: 20px;
  font-weight: 600;
  line-height: 27.24px;
  text-align: left;
  color: #4E3000;
}
td{
  font-family: OpenSans;
  font-size: 30px;
  font-weight: 400;
  line-height: 40.85px;
  text-align: left;
}
.table__th{
  text-align: end;
}
.table__td{
  display: flex;
  justify-content: flex-end;
}
.absent{
  width: 59px;
  height: 59px;
  border-radius: 30px;
  background: #80BB00;
}
.present{
  width: 59px;
  height: 59px;
  border-radius: 30px;
  background: #EC5937;
}
@font-face {
	font-family: 'OpenSans'; 
	src: url(/assets/Font/OpenSans.ttf); 
}
</style>
