<template>
  <li class="ui-state-default">
    <div class="checkbox">
        <div class="input-group">
            <label v-if="!isEditing" class="form-control" style="border:0px">
                <input @change="$emit('changeStatus', todoItem)" type="checkbox" name="Completed" v-model="todoItem.Completed">
                {{todoItem.Description}}
            </label>
            <input v-if="isEditing" type="text" class="form-control" v-model="todoItem.Description">
            <span class="input-group-btn float-right">
                <button v-if="!isEditing" @click="setEditing(true)" class="btn btn-primary">Edit</button>
                <button v-if="!isEditing" @click="$emit('remove', todoItem)" class="btn btn-danger">Remove</button>
                <button v-if="isEditing" @click="setEditing(false)" class="btn btn-success">Save</button>
                <button v-if="isEditing" @click="discardChanges()" class="btn btn-warning">Cancel</button>
            </span>
        </div>
    </div>
    
  </li>
</template>

<script>
export default {
  created(){
      this.todoItemBeforeEdit = Object.assign({}, this.todoItem)
  },
  props: {
    todoItem: {
        type: Object,
        required: true
    }
  },
  data(){
      return {
          isEditing: false,
          todoItemBeforeEdit:{
              
          }
      }
  },
  methods: {
      setEditing(isEditing){
          this.isEditing = isEditing;
      },
      discardChanges(){
          Object.assign(this.todoItem, this.todoItemBeforeEdit)
          this.setEditing(false);
      }
  }
}
</script>