<template>
  <div>
    <NavbarPage />
    <div class="input-group flex-nowrap">
      <input
        type="text"
        class="form-control"
        placeholder="Write something ..."
        v-model="new_work"
      />
      <button
        class="btn btn-success"
        @click="todos.push({ todo: new_work, status: false })"
      >
        Thêm mới
      </button>
    </div>
    <table class="table">
      <thead>
        <tr>
          <th></th>
          <th scope="col">Contents</th>
          <th scope="col">Actions</th>
        </tr>
      </thead>
      <tbody v-for="(item, index) in todos" v-bind:key="index">
        <tr>
          <td>
            <input type="checkbox" v-model="item.status" />
          </td>
          <td :class="{ done: item.status }">{{ item.todo }}</td>
          <td>
            <button
              type="button"
              class="btn btn-danger btn-sm"
              @click="deleteContent(index)"
            >
              Delete
            </button>
            <button
              type="button"
              @click="editTodo(item.todo)"
              class="btn btn-primary btn-sm"
              data-bs-toggle="modal"
              data-bs-target="#exampleModal"
              data-bs-whatever="@mdo"
            >
              Update
            </button>
          </td>
        </tr>
      </tbody>
    </table>
    <div
      class="modal fade"
      id="exampleModal"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-5" id="exampleModalLabel">New message</h1>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">
            <form>
              <div class="mb-3">
                <label for="recipient-name" class="col-form-label"
                  >Content</label
                >
                <input
                  type="text"
                  class="form-control"
                  id="recipient-name"
                  v-model="edit_work.todo"
                />
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button class="btn btn-secondary" data-bs-dismiss="modal">
              Close
            </button>
            <button
              class="btn btn-primary"
              data-bs-dismiss="modal"
              @click="() => editContent(edit_work.index)"
            >
              Save
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import NavbarPage from "./components/NavbarPage.vue";

export default {
  data() {
    return {
      new_work: "",
      edit_work: {
        todo: "",
        index: "",
      },
      todos: [],
    };
  },

  components: {
    NavbarPage,
  },

  watch: {
    todos: {
      handler() {
        localStorage.setItem("todos", JSON.stringify(this.todos));
      },
      deep: true,
    },
  },

  mounted() {
    if (localStorage.getItem("todos")) {
      this.todos = JSON.parse(localStorage.getItem("todos"));
    }
  },

  methods: {
    editContent(index) {
      this.todos[index].todo = this.edit_work.todo;
    },
    deleteContent(index) {
      this.todos.splice(index, 1);
    },
    editTodo(todo) {
      for (let i = 0; i < this.todos.length; i++) {
        if (this.todos[i].todo === todo) {
          this.edit_work = {
            todo: todo,
            index: i,
          };
        }
      }
    },
  },
};
</script>

<style>
.table {
  margin: 10px;
  text-align: center;
}

.done {
  text-decoration: line-through;
  color: gray;
}

.container-fluid {
  margin: 5px 0;
}
</style>
