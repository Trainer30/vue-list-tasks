<template>
  <div>
    <input class="input-task text-size18" type="text" placeholder="What do you have to do?" @keyup.enter="saveNewTask">
    <div class="py20 text-right d-flex-m flex-space-between flex-align-items-center" v-show="listNoEmpty" id="actions-tasks">
        <div>
            <button v-show="completedTasks" class="btn btn-clear" @click="clearCompleted">Clear completed</button>
        </div>

        <div class="btn-group">
          <span style="margin-right: 1rem;">filter :</span>
          <button type="button"  class="btn" :class="{'btn-active': filter === 'all'}" @click="filter ='all'">All</button>
          <button type="button"  class="btn" :class="{'btn-active': filter === 'incomplete'}" @click="filter ='incomplete'">Incomplete</button>
          <button type="button"  class="btn" :class="{'btn-active': filter === 'completed'}" @click="filter ='completed'">Completed</button>
        </div>
        
    </div>
    <div class="list-tasks" v-for="(item,index) in filterTasks" :key="index">
      <div class="list-task-item text-size16 d-flex-m flex-row flex-nowrap flex-space-between flex-align-items-center p20">
        <div class="d-flex-m flex-space-between flex-align-items-center" v-show="!item.editing">
          <input type="checkbox" :checked="item.completed" class="margin-right" @change="changeStateComplete($event,item)">
          <div class="text-task text-size18" :class="{completed:item.completed}" @dblclick="editTask(item)">
            {{  item.description }}
          </div>
        </div>
        <input v-show="item.editing" class="input-edit text-size18" type="text" v-model="draft" @keyup.enter="saveChanges(item)" @blur="saveChanges(item)" @keyup.esc="cancelChanges(item)">
        <div class="actions-task" :class="{grow1: item.editing}">
            <span class="size-times" @click="deleteTask(index)">&times;</span>
        </div>
      </div>
    </div>
    <div class="d-flex-m flex-row flex-nowrap flex-space-between flex-align-items-center p20 text-size16" v-show="listNoEmpty">
      <div>
        <label>
          <input type="checkbox" :checked="noPendingTasks" @change="completeAllTasks">
          Check all
        </label>
      </div>
      <div>
        {{remainingTasks}} pending tasks
      </div>

    </div>
    
  </div>
</template>

<script>
export default {
  name: 'list-tasks',
  data () {
    return {
      filter: 'all',
      id: 3,
      list_tasks:[
        {
          id: 0,
          description: 'Descripción de la ta tarea 1',
          completed: false,
          editing: false
        },
        {
          id: 1,
          description: 'Descripción de la ta tarea 2',
          completed: true,
          editing: false
        },
        {
          id: 2,
          description: 'Descripción de la ta tarea 3',
          completed: false,
          editing: false
        }
      ],
      draft: '',
      descriptionCache: '' 
    }
  },
  // directives:{
  //   focus: {
  //     inserted: function (el) {
  //       el.focus()
  //     }
  //   }
  // },
  methods:{
    editTask(item){
      console.log("entro");
      this.draft = item.description
      this.descriptionCache = item.description
      this.list_tasks.forEach( task => {
        if(item.id === task.id) 
          task.editing = true
        else
          task.editing = false
      })
      console.log("termino");
    },
    saveChanges(item){
      let newDescription = this.draft.trim()
      if(newDescription.length !== 0){
          // this.list_tasks.forEach(task =>{
          //   if(task.id === item.id){
          //     task.description = newDescription
          //     task.editing = false
          //   }
          // })
          item.description = this.draft
      }
      item.editing = false
    },
    cancelChanges(item){
      this.draft = this.descriptionCache
      item.editing = false
    },
    changeStateComplete(e,item){
      item.completed = e.target.checked
    },
    saveNewTask(e){
        let newTask = e.target.value.trim()
        if(newTask.length > 0){
          this.list_tasks.push({
              id : this.id,
              description: newTask,
              editing: false,
              completed: false
          })
          this.id++
          e.target.value = ''
        }
    },
    deleteTask(index){
      this.list_tasks.splice(index,1)
    },
    completeAllTasks(e){
      this.list_tasks.forEach(task =>{
        task.completed = e.target.checked 
      })
    },
    clearCompleted(){
      this.list_tasks = this.list_tasks.filter(task => !task.completed)
    }
  },
  computed:{
    listNoEmpty(){
      return this.list_tasks.length
    },
    remainingTasks(){
      return this.list_tasks.filter(task => !task.completed).length
    },
    noPendingTasks(){
      return this.listNoEmpty <= 0 ? false 
              : this.remainingTasks === 0 ? true : false
    },
    completedTasks(){
      return this.listNoEmpty <= 0 ? false
              : this.list_tasks.filter(task => task.completed).length
    },
    filterTasks(){
      if(this.filter === 'incomplete'){
          return this.list_tasks.filter(task => !task.completed)
      }else if(this.filter === 'completed'){
          return this.list_tasks.filter(task => task.completed)
      }else{
        return this.list_tasks
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
/*style for text*/
.text-right{
  text-align: right;
}

.text-size16{
  font-size: 16px;
}
.text-size18{
  font-size: 18px;
}

.input-task{
  width: 100%;
  color: var(--color-input);
  padding: 10px 15px;
  margin-bottom: 2rem;
}

.d-flex-m{
  display: flex;
}

.flex-row{
  flex-direction: row;
}

.flex-nowrap{
  flex-wrap: nowrap;
}

.flex-wrap{
  flex-wrap: wrap;
}

.flex-justify-end {
  justify-content: flex-end;
}

.flex-align-items-center{
  align-items: center;
}

.flex-justify-content-center{
  justify-content: center;
}

.flex-space-between{
  justify-content: space-between;
}

/*Styles icon delete*/
.size-times{
  font-size: 2rem;
}

/*Styles for list of tasks*/
.list-tasks{
  width: 100%;
}

/*Styles for item task*/
.list-task-item{
  border: 1px solid #ccc;
}
/*section of actions*/ 
.actions-task{
  margin-left: 1rem;
  cursor:pointer;
}


/*paddings*/
.p20{
  padding: 20px;
}
.py20{
  padding: 20px 0;
}

/* Margins */
.margin-right{
  margin-right: 1rem;
}

/*Styles for buttons*/
.btn{
  padding: 10px 1rem;
  transform: translateY(0);
  transition: transform .5s;
  border-style: solid;
  border-width: 1px;
}
.btn:active{
  transform: translateY(-10px);
}
.btn.btn-clear{
  background-color: rgb(212, 75, 75);
  border-color: rgb(209, 61, 61);
  color: #fff;
}
.btn-group .btn.btn-active{
  background-color: rgb(52, 250, 95); 
  border-color: rgba(52, 250, 95);
  /* enable-background: 0; */
  position:relative;
  /* will-change: transform; */
}
.btn-active:focus, .btn-active:active{
  outline: 0;
}
.btn-group .btn{
  background-color: rgb(42, 53, 211);
  color: #fff;
  border-color: rgb(28, 40, 206);
}


/* styles tasks completed */
.completed{
  text-decoration: line-through;
  color: rgb(177, 174, 174);
}

 .input-edit{
   flex-grow: 18;
   padding: 10px 1rem;
   color: var(--color-input);
 }
 .grow1{
   flex-grow: 1;
 }

 @media(max-width: 759px){
   #actions-tasks.flex-space-between{
     flex-direction: column;
     align-items: start;
   }
   .btn-group{
     margin-top: 1rem;
   }
 }
 @media(max-width: 591px){
   .container{
     width: 98%;
   }
   /* #actions-tasks.flex-space-between{
     flex-direction: column;
     align-items: start;
   }
   .btn-group{
     margin-top: 1rem;
   } */
 }
</style>
