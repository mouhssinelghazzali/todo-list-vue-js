<template>
    <div>
        <h2 :class="{
            'is-completed':todo.completed
        }">{{todo.title}}</h2>
        <div class="text-right">
                <button @click="updateMyTodo" class="btn btn-info btn-sm mr-1">Edit</button>
                <button @click="showAlertConfirm" class="btn btn-danger btn-sm">Delete</button>
            </div>
    </div>
</template>

<script>
export default {
    props:['todo'],

    methods:{
         deleteTodo(id)
        {
            this.$emit('deteleTodo',this.todo.id);
        },
        showAlert(){
            this.$swal('Hello Vue world!!!');
        },
        updateMyTodo()
        {
            this.$emit('updateTodo',this.todo)
        },
        showAlertConfirm(){
            this.$swal({
              title: 'Are you sure?',
              text: "You won't be able to revert this!",
              type: 'warning',
              showCancelButton: true,
              confirmButtonColor: '#3085d6',
              cancelButtonColor: '#d33',
              confirmButtonText: 'Yes, delete it!'
            }).then((result) => {
              if (result.value) {
                this.$swal(
                  'Deleted!',
                  'Your file has been deleted.',
                  'success'
                )
                this.deleteTodo(this.todo.id)
              }
            });
        }
    }
}
</script>

<style>
.is-completed
{
    text-decoration: line-through;
}
</style>
