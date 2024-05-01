<!-- Main.vue -->
<template>
  <div class="box">
    <h1 v-show="activities.length > 0">To-Do list</h1>
    <p v-show="activities.length === 0">
      No activities left. Add a new one!
    </p>
    <transition-group name="activity" tag="ul">
      <li
        v-for="(activity, index) in activities.filter(a =>!a.complete)"
        :key="activity.id"
        class="activity-item"
      >
        <transition name="button">
          <button class="edit" v-if="editingActivity!== activity" @click="editActivity(activity, index)">
            Edit
          </button>
        </transition>
        <transition name="button">
          <button class="delete" @click="deleteActivity(index)">x</button>
        </transition>
        <transition name="activity-inner">
          <div v-if="editingActivity === activity" class="activity-inner">
            <input type="text" v-model="activity.name" />
            <button class="edit" @click="saveActivity(activity)">Save</button>
            <button class="edit" @click="cancelEdit">Cancel</button>
          </div>
        </transition>
        {{ activity.name }}
        <input type="checkbox" v-model="activity.complete" />
      </li>
    </transition-group>
    <input
      type="text"
      v-model="newActivity"
      placeholder="Add New Activities"
    />
    <button @click="addActivity">Add</button>

    <h2 v-show="activities.filter(a => a.complete).length > 0">
      Completed
    </h2>
    <transition-group name="activity" tag="ul">
      <li
        v-for="(activity, index) in activities.filter(a => a.complete)"
        :key="activity.id"
        class="activity-item"
      >
        <transition name="button">
          <button class="edit" v-if="editingActivity!== activity" @click="editActivity(activity, index)">
            Edit
          </button>
        </transition>
        <transition name="button">
          <button class="delete" @click="deleteActivity(index)">x</button>
        </transition>
        <transition name="activity-inner">
          <div v-if="editingActivity === activity" class="activity-inner">
            <input  type="text" v-model="activity.name" />
            <button class="edit" @click="saveActivity(activity)">Save</button>
            <button class="edit" @click="cancelEdit">Cancel</button>
          </div>
        </transition>
        {{ activity.name }}
        <input type="checkbox" v-model="activity.complete" />
      </li>
    </transition-group>
  </div>
</template>

<script>
export default {
  data() {
    return {
      activities: [],
      newActivity: "",
      editingActivity: null,
    };
  },
  methods: {
    addActivity() {
      if (this.newActivity.trim()) {
        this.activities.push({
          name: this.newActivity,
          complete: false,
          id: this.activities.length + 1,
        });
        this.newActivity = "";
      }
    },
    deleteActivity(index) {
      this.activities.splice(index, 1);
    },
    editActivity(activity, index) {
      this.editingActivity = activity;
    },
    saveActivity(activity) {
      this.editingActivity = null;
      alert("New Data Saved");
    },
    cancelEdit() {
      this.editingActivity = null;
    },
  },
};
</script>

<style scoped>

.activity-enter-active,
.activity-leave-active {
  transition: all 0.5s ease;
}
.activity-enter-from,
.activity-leave-to {
  opacity: 0;
  transform: translateX(30px);
}

.activity-inner-enter-active,
.activity-inner-leave-active {
  transition: all 0.5s ease;
}
.activity-inner-enter-from,
.activity-inner-leave-to {
  opacity: 0;
  transform: translateX(30px);
}

.button-enter-active,
.button-leave-active {
  transition: opacity 0.3s;
}
.button-enter,
.button-leave-to {
  opacity: 0;
}

.activity-item {
  position: relative;
}

.activity-inner {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>