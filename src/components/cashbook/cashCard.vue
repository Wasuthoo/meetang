<template>
  <v-card class="ma-5 bg-blue-lighten-4" @click="onClickCard(transaction)" v-for="transaction in savingGoal"
    :key="transaction.id">
    <v-card-title>{{ transaction.name }}</v-card-title>
    <v-card-text class="mx-2">{{ transaction.amount }}</v-card-text>
    <v-card-subtitle class="mx-2">{{ transaction.date }}</v-card-subtitle>
    <v-card-text class="mx-2">{{ transaction.description }}</v-card-text>
  </v-card>

  <!-- show data form -->
  <v-dialog v-model="showdialog" max-width="500px">
    <v-card>
      <v-card-title>
        <span class="headline pa-6"> {{ selectedGoal.name }} </span>
      </v-card-title>

      <v-card-text>
        <v-container>
          <v-row>
            <v-col cols="12">
              <v-text-field v-model="selectedGoal.name" id="name" label="Name" :readonly="!editMode"></v-text-field>
            </v-col>
            <v-col cols="12">
              <v-text-field v-model="selectedGoal.amount" id="amount" label="Amount"
                :readonly="!editMode"></v-text-field>
            </v-col>
            <v-col cols="12">
              <v-text-field v-model="selectedGoal.date" id="date" label="Date" :readonly="!editMode"></v-text-field>
            </v-col>
            <v-col cols="12">
              <v-text-field v-model="selectedGoal.description" id="description" label="Description"
                :readonly="!editMode"></v-text-field>
            </v-col>
          </v-row>
        </v-container>
      </v-card-text>

      <v-card-actions v-if="editMode">
        <v-spacer></v-spacer>
        <v-btn color="red darken-1" text @click="onDeleteGoal">DELETE</v-btn>
        <v-btn color="blue darken-1" text @click="onCancelGoal">CANCLE</v-btn>
        <v-btn color="blue darken-1" text @click="onSaveGoal">SAVE</v-btn>
      </v-card-actions>

      <v-card-actions v-else>
        <v-spacer></v-spacer>
        <v-btn color="blue darken-1" text @click="editMode = true">EDIT</v-btn>
        <v-btn color="blue darken-1" text @click="showdialog = false">CLOSE</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>


</template>

<script>
export default {
  name: "goalCard",
  props: {
    savingGoal: {
      type: Array,
      required: true,
    },
  },
  methods: {
    onClickCard(goal) {
      this.selectedGoal = { ...goal }
      this.showdialog = true;
    },
    onSaveGoal() {
      let index = this.savingGoal.findIndex(goal => goal.id === this.selectedGoal.id);
      this.savingGoal[index] = { ...this.selectedGoal };
      this.editMode = false;
      this.showdialog = false;
    },
    onCancelGoal() {
      this.editMode = false;
      this.showdialog = false;
    },
    onDeleteGoal() {
      let index = this.savingGoal.findIndex(goal => goal.id === this.selectedGoal.id);
      this.savingGoal.splice(index, 1);
      this.editMode = false;
      this.showdialog = false;
    },
  },
  data() {
    return {
      showdialog: false,
      editMode: false,
      selectedGoal: {},
    };
  },
};
</script>
