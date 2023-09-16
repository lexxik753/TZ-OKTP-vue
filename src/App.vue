<script setup>
import { computed, ref, watch } from 'vue'

const EMAIL_REGEXP = /^(([^<>()[\].,;:\s@"]+(\.[^<>()[\].,;:\s@"]+)*)|(".+"))@(([^<>()[\].,;:\s@"]+\.)+[^<>()[\].,;:\s@"]{2,})$/iu;
const PHONE_REGEXP = /^\+\d\(\d{3}\)\d{3}-\d{2}-\d{2}$/;

const name = ref(null)
const showName = ref('')
const errorName = ref('')
const isErrorName = ref(false)

const email = ref(null)
const showEmail = ref('')
const errorEmail = ref('')
const isErrorEmail = ref(false)

const phone = ref(null)
const showPhone = ref('')
const errorPhone = ref('')
const isErrorPhone = ref(false)
const sumOfPhone = ref(0)

const date = ref('')

const displayContent = ref(false)

function checkName(name) {
  if (name == '') {
    errorName.value = 'Поле имя незаполнено'
    isErrorName.value = true
  } else {
    isErrorName.value = false
  }
}

function checkEmail(email) {
  if (email == '') {
    errorEmail.value = 'Поле почты незаполнено'
    isErrorEmail.value = true
  } else if (email != null && !EMAIL_REGEXP.test(email)) {
    errorEmail.value = 'Неверно указан формат почты'
    isErrorEmail.value = true
  } else {
    isErrorEmail.value = false
  }
}

function checkPhone(phone) {
  if (phone == '') {
    errorPhone.value = 'Поле телефон не заполнено'
    isErrorPhone.value = true
  } else if (phone != null && !PHONE_REGEXP.test(phone)) {
    errorPhone.value = 'Неверно указан формат телефона'
    isErrorPhone.value = true
  } else {
    isErrorPhone.value = false
  }
}

function clickButton() {
  if (isErrorName.value || isErrorEmail.value || isErrorPhone.value) {
    return
  }

  let valName = name.value.replace(/\s+/g, ' ').trim()
  valName = valName.split(' ')
  showName.value = valName.map((el) => el.charAt(0).toUpperCase() + el.slice(1)).join(' ')
  showEmail.value = email.value.split('@');

  let toSumOf = phone.value.replace(/[^0-9]/g, '')
  sumOfPhone.value = Array.from(toSumOf).map(v => parseFloat(v)).reduce((acc, num) => acc + num, 0)
  showPhone.value = phone.value

  date.value = new Date().toLocaleDateString()

  displayContent.value = true
}

function clearAll() {
  displayContent.value = false
  name.value = null
  email.value = null
  phone.value = null
}

watch(name, checkName)
watch(email, checkEmail)
watch(phone, checkPhone)

</script>

<template>
  <div class="flex flex-col bg-zinc-400 wrapper w-full h-screen">
    <header class="w-screen-xl bg-black text-white flex mb-24">
      <h1 class="px-8 py-4 text-xl">ТЗ</h1>
      <div class="container mx-auto flex justify-end space-x-7 p-4">
        <button
          class="self-center border-b-2 white transition ease-in-out delay-150 hover:-translate-y-1 hover:scale-110 hover:bg-indigo-500 duration-700">Страница
          1</button>
        <button class="self-center hover:scale-110 active:-translate-y-2 duration-700">Страница 2</button>
        <button class="self-center hover:skew-y-12 active:-translate-x-3">Страница 3</button>
        <button class="self-center hover:rotate-180 active:translate-x-3">Страница 4</button>
        <button class="self-center hover:scale-110 active:translate-y-6">Страница 5</button>
      </div>
    </header>
    <main>
      <div class="grid grid-cols-3 grid-flow-row mx-[10em]">
        <section class="col-span-2 flex flex-col h-[334px]">
          <h2 class="text-4xl text-white pb-5">Тестовое задание</h2>
          <p v-if="!displayContent" id="parag"
            class="pt-0 mt-0 animate__animated animate__slideInDown animate__slower text-white h-full">
            Lorem ipsum dolor, sit
            amet consectetur adipisicing elit. Ex cupiditate eveniet eius ratione, nulla delectus tenetur
            velit facere minus magni libero provident repellendus. Quod quisquam ipsum quaerat vero?
            Molestiae, nobis?
          </p>
          <div v-else class="grid grow grid-rows-3 pr-10">
            <div class="grid grid-cols-2">
              <div class="text-white text-base">Привет {{ showName }}</div>
              <div class="text-white text-base text-right">Моя почта {{ showEmail[0] }} в сервисе {{ showEmail[1] }}</div>
              <div class="text-white text-base">Сумма цифр в моем телефоне: {{ showPhone }} равна {{ sumOfPhone }}</div>
              <div class="text-white text-base text-right">Длина моего имени {{ showName.length % 2 == 0 ? 'четная' :
                'нечетная' }}
              </div>
            </div>
            <div class="text-white text-base mt-8">{{ date }}</div>
            <button @click="clearAll" class="clear__button">Сброс</button>
          </div>
        </section>
        <aside class="grid auto-rows-2 auto-cols-auto w-80 h-[334px] bg-gray-200">
          <div class="flex flex-col w-full bg-white">
            <h3 class="text-lg text-center font-bold h-14 bg-zinc-100 py-4">Введите <span class="text-orange-600"> Ваши
              </span> данные</h3>
            <div class="px-8 pt-4 pb-2 relative">
              <input v-model="name" :class="{ 'border-red-500': isErrorName }"
                class="focus:outline-none border-b-2 p-2 w-full" type="text" placeholder="Имя" required>
              <p v-show="isErrorName" style="display: none;" class="text-red-500 text-xs italic w-full absolute">{{
                errorName }}</p>
            </div>
            <div class="px-8 pt-4 pb-2 relative">
              <input v-model="email" :class="{ 'border-red-500': isErrorEmail }"
                class="focus:outline-none border-b-2 p-2 w-full" type="email" placeholder="E-mail" required>
              <p v-show="isErrorEmail" style="display: none;" class="text-red-500 text-xs italic w-full absolute">{{
                errorEmail }}</p>
            </div>
            <div class="px-8 py-4 relative">
              <input v-model="phone" :class="{ 'border-red-500': isErrorPhone }"
                class="focus:outline-none border-b-2 p-2 w-full" type="text" placeholder="Телефон +7(999)999-99-99"
                required>
              <p v-show="isErrorPhone" style="display: none;" class="text-red-500 text-xs italic w-full absolute">{{
                errorPhone }}</p>
            </div>
            <button @click="clickButton"
              class="h-full w-full mt-6 text-center text-white bg-orange-400">Отправить</button>
          </div>
        </aside>
      </div>
    </main>
    <footer class="mt-auto bg-white w-full p-3">
      <nav>
        <div class="container flex justify-end space-x-7 ml-auto mr-9 pb-5 pt-4">
          <a href="https://vk.com/" target="_blank"><img src="img/icons8-vk-circled.svg" alt="ссылка на Вконтакте"></a>
          <a href="https://ru.pinterest.com/" target="_blank"><img src="img/icons8-pinterest.svg"
              alt="ссылка на Pinterest"></a>
          <a href="https://discord.com/" target="_blank"><img src="img/icons8-discord.svg" alt="ссылка на Discord"></a>
        </div>
      </nav>
    </footer>
  </div>
</template>

<style scoped>
.clear__button {
  width: 100px;
  height: 40px;
  color: white;
  border: 1px solid white;
  margin-top: 40px;
}
</style>
