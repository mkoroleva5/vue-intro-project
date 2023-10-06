<script setup lang="ts">
import { ref, type Ref } from 'vue'
import deleteIcon from '@/assets/x.svg'

interface FormState {
  email: string
  password: string
  role: string
  terms: boolean
  skills: string[]
  tempSkill: string
  isSkillPushed: boolean
  passwordError: string
}

const formState: Ref<FormState> = ref({
  email: '',
  password: '',
  role: 'developer',
  terms: false,
  skills: [],
  tempSkill: '',
  isSkillPushed: false,
  passwordError: ''
})

const addSkill = (e: KeyboardEvent | MouseEvent) => {
  const skill = formState.value.tempSkill
  const skillsArray = formState.value.skills

  if ((e instanceof KeyboardEvent && e.key === 'Enter') || e instanceof MouseEvent) {
    if (skill && !skillsArray.includes(skill)) {
      formState.value.skills.push(skill)
      formState.value.isSkillPushed = true
    }
    formState.value.tempSkill = ''
    setTimeout(() => (formState.value.isSkillPushed = false), 1000)
  }
}

const handleSubmit = () => {
  if (formState.value.password.length < 8) {
    formState.value.passwordError = 'Password must be at least 8 chars long'
  } else {
    formState.value = {
      email: '',
      password: '',
      role: 'developer',
      terms: false,
      skills: [],
      tempSkill: '',
      isSkillPushed: false,
      passwordError: ''
    }
  }
}
</script>

<template>
  <form class="form" @submit.prevent="handleSubmit" @keydown.enter.prevent>
    <label class="label">Email:</label>
    <input class="input" type="email" v-model="formState.email" required />

    <label class="label">Password:</label>
    <input class="input" type="password" v-model="formState.password" required />
    <div v-if="formState.passwordError" class="error">{{ formState.passwordError }}</div>

    <label class="label">Role:</label>
    <select class="select" v-model="formState.role">
      <option value="developer">Web Developer</option>
      <option value="designer">Web Designer</option>
    </select>

    <label class="label">Skills:</label>
    <div class="checkboxes">
      <input class="checkbox" type="checkbox" value="HTML" v-model="formState.skills" />
      <label class="label">HTML</label>

      <input class="checkbox" type="checkbox" value="CSS" v-model="formState.skills" />
      <label class="label">CSS</label>

      <input class="checkbox" type="checkbox" value="JavaScript" v-model="formState.skills" />
      <label class="label">JavaScript</label>
    </div>

    <div class="skills-wrapper">
      <div v-for="skill in formState.skills" :key="skill" class="skill">
        {{ skill }}
        <input
          type="checkbox"
          :value="skill"
          v-model="formState.skills"
          class="delete-input"
          checked
        />
        <img :src="deleteIcon" alt="Delete" class="delete-icon" />
      </div>
    </div>

    <label class="label">More skills:</label>
    <div class="skills-wrapper">
      <input class="input" type="text" v-model="formState.tempSkill" @keyup="addSkill" />
      <button
        type="button"
        class="enter-button"
        :class="{ pressed: formState.isSkillPushed }"
        @click="addSkill"
      >
        enter
      </button>
    </div>

    <div class="checkboxes">
      <input class="checkbox" type="checkbox" v-model="formState.terms" required />
      <label class="label">Accept terms and conditions</label>
    </div>

    <button class="submit-button">Create an account</button>
  </form>
</template>

<style scoped>
.form {
  max-width: 500px;
  margin: 10px auto;
  padding: 20px 40px;
  display: flex;
  flex-direction: column;
  color: #444;
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 2px 2px 10px #ccc;
}

.label {
  color: #aaa;
  display: inline-block;
  margin: 20px 0 5px;
  font-size: 12px;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: bold;
}

.error {
  font-size: 12px;
  color: red;
}
.input,
.select {
  display: block;
  padding: 10px 6px;
  width: 100%;
  box-sizing: border-box;
  border: none;
  border-bottom: 1px solid #ddd;
  color: #555;
}

.input:focus,
.select:focus {
  outline: 1px solid #ccc;
  border-radius: 5px;
}
.checkboxes {
  display: flex;
  gap: 10px;
  margin-bottom: 10px;
}

.checkbox {
  display: inline-block;
  width: 16px;
  margin-top: 15px;
}

.skills-wrapper {
  position: relative;
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.skill {
  position: relative;
  width: fit-content;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 8px;
  padding: 5px 10px;
  border-radius: 10px;
  background-color: #eee;
  font-size: 12px;
  letter-spacing: 1px;
  font-weight: bold;
  color: #777;
}

.delete-input {
  z-index: 3;
  opacity: 0;
  cursor: pointer;
}
.delete-icon {
  position: absolute;
  top: 7px;
  right: 9px;
}
.enter-button {
  position: absolute;
  top: 8px;
  right: 10px;
  width: 40px;
  display: block;
  border-radius: 2px;
  padding: 2px 7px;
  background-color: #eee;
  color: #a5a5a5;
  border: solid 3px #ccc;
  border-width: 1px 1px 6px 1px;
  font-family: menlo, monospace;
  font-size: 9px;
  cursor: pointer;
}

.pressed {
  animation-name: press;
  animation-duration: 0.3s;
  animation-iteration-count: 1;
}

@keyframes press {
  0% {
    border-width: 1px 1px 6px 1px;
    height: 21px;
    top: 8px;
    bottom: 0;
  }

  50% {
    border-width: 1px 1px 2px 1px;
    height: 17px;
    top: 13px;
    bottom: 0;
  }

  100% {
    border-width: 1px 1px 6px 1px;
    height: 21px;
    top: 8px;
    bottom: 0;
  }
}

.submit-button {
  width: fit-content;
  background: #0b6dff;
  opacity: 0.8;
  border: 0;
  padding: 10px 20px;
  margin: auto;
  margin-top: 20px;
  margin-bottom: 10px;
  color: white;
  font-size: 14px;
  border-radius: 20px;
  text-align: center;
  letter-spacing: 1px;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.2s ease-in;
}

.submit-button:hover {
  opacity: 1;
}
</style>
