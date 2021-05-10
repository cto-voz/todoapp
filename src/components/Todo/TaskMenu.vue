<template>
  <div>

    <v-menu
        bottom
        left
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
            color="primary"
            icon
            v-bind="attrs"
            v-on="on"
        >
          <v-icon>mdi-dots-vertical</v-icon>
        </v-btn>
      </template>

      <v-list>
        <v-list-item
            v-for="(item, index) in items"
            :key="index"
            @click="handleClick(index)"
        >
          <v-list-item-icon>
            <v-icon v-text="item.icon"></v-icon>
          </v-list-item-icon>
          <v-list-item-title>{{ item.title }}</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-menu>

    <dialog-delete
        v-if="dialogs.delete"
        @close="dialogs.delete = false"
        :task="task"
    />
    <dialog-edit
        v-if="dialogs.edit"
        @close="dialogs.edit = false"
        :task="task"
    />
    <dialog-due-date
        v-if="dialogs.dueDate"
        @close="dialogs.dueDate = false"
        :task="task"
    />
  </div>
</template>

<script>
export default {
  props: ["task"],
  data: () => ({
    dialogs: {
      edit: false,
      delete: false,
      dueDate: false,
    },
    items: [
      {
        title: 'Edit', icon: 'mdi-pencil',
        click() {
          this.dialogs.edit = true
        }
      },
      {
        title: 'Due date', icon: 'mdi-calendar',
        click() {
          this.dialogs.dueDate = true
        }
      },
      {
        title: 'Delete', icon: 'mdi-delete',
        click() {
          this.dialogs.delete = true
        }
      },
      {
        title: 'Sort', icon: 'mdi-drag-horizontal-variant',
        click() {
          if (!this.$store.state.search) {
            this.$store.commit('toggleSorting')
          } else {
            this.$store.commit('showSnackbar', 'Close the search!')
          }
        }
      },
    ],
  }),
  methods: {
    handleClick(index) {
      this.items[index].click.call(this)
    }
  },
  components: {
    'dialog-delete': require('./Dialogs/DialogDelete').default,
    'dialog-edit': require('./Dialogs/DialogEdit').default,
    'dialog-due-date': require('./Dialogs/DialogDueDate').default,
  }
}
</script>

<style scoped>

</style>