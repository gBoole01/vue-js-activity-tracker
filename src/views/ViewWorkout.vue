<template>
  <div class="max-w-screen-sm mx-auto px-4 py-10">
    <!-- App Msg -->
    <div
      v-if="statusMsg || errorMsg"
      class="mb-10 p-4 rounded-md shadow-md bg-light-grey"
    >
      <p class="text-at-light-green">
        {{ statusMsg }}
      </p>
      <p class="text-red-500">
        {{ errorMsg }}
      </p>
    </div>

    <div v-if="dataLoaded">
      <!-- General Workout Info -->
      <div
        class="flex flex-col items-center p-8 rounded-md shadow-md bg-light-grey relative"
      >
        <div v-if="user" class="flex absolute left-2 top-2 gap-x-2">
          <div
            @click="editMode"
            class="h-7 w-7 rounded-full flex justify-center items-center cursor-pointer bg-at-light-green shadow-lg"
          >
            <img src="@/assets/images/pencil-light.png" class="h-3.5 w-auto" />
          </div>
          <div
            @click="deleteWorkout"
            class="h-7 w-7 rounded-full flex justify-center items-center cursor-pointer bg-at-light-green shadow-lg"
          >
            <img src="@/assets/images/trash-light.png" class="h-3.5 w-auto" />
          </div>
        </div>

        <img
          v-if="data.workoutType === 'cardio'"
          src="@/assets/images/running-light-green.png"
          class="h-24 w-auto"
        />
        <img
          v-if="data.workoutType === 'strength'"
          src="@/assets/images/dumbbell-light-green.png"
          class="h-24 w-auto"
        />

        <span
          class="mt-6 pointer-events-nonepy-1.5 px-5 text-xs text-white bg-at-light-green rounded-lg shadow-md"
        >
          {{ data.workoutType }}
        </span>

        <div class="w-full mt-6">
          <input
            v-if="edit"
            type="text"
            class="p-2 w-full text-gray-500 focus:outline-none"
            v-model="data.workoutName"
          />
          <h1 v-else class="text-at-light-green text-2xl text-center">
            {{ data.workoutName }}
          </h1>
        </div>
      </div>

      <!-- Exercises -->
      <div
        class="mt-10 p-8 rounded-md flex flex-col item-center bg-light-grey shadow-md"
      >
        <!-- Strength Training -->
        <div
          v-if="data.workoutType === 'strength'"
          class="flex flex-col gap-y-4 <-full"
        >
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative sm:flex-row"
            v-for="(item, index) in data.exercises"
            :key="index"
          >
            <div class="flex flex-2 flex-col md:w-1/3">
              <label
                for="exercise-name"
                class="mb-1 text-sm text-at-light-green"
              >
                Exercise
              </label>
              <input
                v-if="edit"
                type="text"
                id="exercise-name"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.exercise"
              />
              <p v-else>{{ item.exercise }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="sets" class="mb-1 text-sm text-at-light-green">
                Sets
              </label>
              <input
                v-if="edit"
                id="sets"
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.sets"
              />
              <p v-else>{{ item.sets }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="reps" class="mb-1 text-sm text-at-light-green">
                Reps
              </label>
              <input
                v-if="edit"
                id="reps"
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.reps"
              />
              <p v-else>{{ item.reps }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="weight" class="mb-1 text-sm text-at-light-green">
                Weight (Kg)
              </label>
              <input
                v-if="edit"
                id="weight"
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.weight"
              />
              <p v-else>{{ item.weight }}</p>
              <img
                v-if="edit"
                @click="deleteExercise(item.id)"
                src="@/assets/images/trash-light-green.png"
                class="absolute h-4 w-auto -left-5 cursor-pointer"
                alt=""
              />
            </div>
          </div>
          <button
            v-if="edit"
            @click="addExercise"
            type="button"
            class="py-2 px-6 rounded-sm self-start text-sm text-white bg-at-light-green duration-200 border-solid border-2 border-transparent hover:border-at-light-green hover:bg-white hover:text-at-light-green"
          >
            Add exercise
          </button>
        </div>

        <!-- Cardio Training -->
        <div
          v-if="data.workoutType === 'cardio'"
          class="flex flex-col gap-y-4 <-full"
        >
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative sm:flex-row"
            v-for="(item, index) in data.exercises"
            :key="index"
          >
            <div class="flex flex-2 flex-col md:w-1/3">
              <label for="cardio-type" class="mb-1 text-sm text-at-light-green">
                Type
              </label>
              <select
                v-if="edit"
                id="cardio-type"
                class="p-2 w-full bg-white text-gray-500 focus:outline-none"
                v-model="item.cardioType"
              >
                <option selected value="#">Select Type</option>
                <option value="run">Run</option>
                <option value="walk">Walk</option>
              </select>
              <p v-else>{{ item.cardioType }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="distance" class="mb-1 text-sm text-at-light-green">
                Distance
              </label>
              <input
                v-if="edit"
                id="distance"
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.distance"
              />
              <p v-else>{{ item.distance }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="duration" class="mb-1 text-sm text-at-light-green">
                Duration
              </label>
              <input
                v-if="edit"
                id="duration"
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.duration"
              />
              <p v-else>{{ item.duration }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="pace" class="mb-1 text-sm text-at-light-green">
                Pace
              </label>
              <input
                v-if="edit"
                id="pace"
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.pace"
              />
              <p v-else>{{ item.pace }}</p>
              <img
                v-if="edit"
                @click="deleteExercise(item.id)"
                src="@/assets/images/trash-light-green.png"
                class="absolute h-4 w-auto -left-5 cursor-pointer"
                alt=""
              />
            </div>
          </div>
          <button
            v-if="edit"
            @click="addExercise"
            type="button"
            class="py-2 px-6 rounded-sm self-start text-sm text-white bg-at-light-green duration-200 border-solid border-2 border-transparent hover:border-at-light-green hover:bg-white hover:text-at-light-green"
          >
            Add exercise
          </button>
        </div>

        <!-- Update -->
        <button
          v-if="edit"
          @click="update"
          type="button"
          class="mt-10 py-2 px-6 rounded-sm self-start text-sm text-white bg-at-light-green duration-200 border-solid border-2 border-transparent hover:border-at-light-green hover:bg-white hover:text-at-light-green"
        >
          Update Workout
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { computed, ref } from 'vue'
import supabase from '../supabase/init'
import { useRoute, useRouter } from 'vue-router'
import store from '../store'
import { uid } from 'uid'

export default {
  name: 'view-workout',
  setup() {
    // Create data / vars
    const data = ref(null)
    const dataLoaded = ref(null)
    const errorMsg = ref(null)
    const statusMsg = ref(null)
    const route = useRoute()
    const router = useRouter()
    const user = computed(() => store.state.user)

    // Get current Id of route
    const currentId = route.params.workoutId

    // Get workout data
    const getData = async () => {
      try {
        const { error, data: workouts } = await supabase
          .from('workouts')
          .select('*')
          .eq('id', currentId)

        if (error) throw error
        data.value = workouts[0]
        dataLoaded.value = true
      } catch (error) {
        errorMsg.value = error.message
        setTimeout(() => {
          errorMsg.value = null
        }, 5000)
      }
    }

    getData()

    // Delete workout
    const deleteWorkout = async () => {
      try {
        const { error } = await supabase
          .from('workouts')
          .delete()
          .eq('id', currentId)

        if (error) throw error
        router.push({ name: 'Home' })
      } catch (error) {
        errorMsg.value = `Error: ${error.message}`
        setTimeout(() => {
          errorMsg.value = null
        }, 5000)
      }
    }

    // Edit mode
    const edit = ref(null)
    const editMode = () => {
      edit.value = !edit.value
    }

    // Add exercise
    const addExercise = () => {
      switch (data.value.workoutType) {
        case 'strength':
          data.value.exercises.push({
            id: uid(),
            exercise: '',
            sets: '',
            reps: '',
            weight: '',
          })
          break
        case 'cardio':
          data.value.exercises.push({
            id: uid(),
            cardioType: '',
            distance: '',
            duration: '',
            pace: '',
          })
          break

        default:
          break
      }
    }

    // Delete exercise
    const deleteExercise = (id) => {
      if (data.value.exercises.length > 1) {
        data.value.exercises = data.value.exercises.filter(
          (exercise) => exercise.id !== id,
        )
        return
      }

      errorMsg.value = 'Error: Workout need at least 1 exercise !'
      setTimeout(() => {
        errorMsg.value = null
      }, 5000)
      return
    }

    // Update Workout
    const update = async () => {
      try {
        const { error } = await supabase
          .from('workouts')
          .update({
            workoutName: data.value.workoutName,
            exercises: data.value.exercises,
          })
          .eq('id', currentId)

        if (error) throw error
        edit.value = false
        statusMsg.value = `Success: Workout Updated`
        setTimeout(() => {
          statusMsg.value = null
        }, 5000)
      } catch (error) {
        errorMsg.value = `Error: ${error.message}`
        setTimeout(() => {
          errorMsg.value = null
        }, 5000)
      }
    }

    return {
      data,
      dataLoaded,
      errorMsg,
      statusMsg,
      user,
      edit,
      editMode,
      addExercise,
      deleteExercise,
      deleteWorkout,
      update,
    }
  },
}
</script>
