<!-- <template>
  <div>login</div>
</template> -->
<script setup>
import { ref } from "vue";
import { toast } from "vue3-toastify";
import "vue3-toastify/dist/index.css";
// eslint-disable-next-line no-undef

const contact = ref({
  fullname: "",
  phone: "",
  email: "",
  address: "",
  laptop: "",
});

const loading = ref(false);
const filePicked = ref(null);

const handleFile = (e) => {
  const file = e.target.files[0];
  if (file) {
    filePicked.value = file.name;
  } else {
    filePicked.value = null;
  }
};

const handleSubmit = async (e) => {
  e.preventDefault();
  const { fullname, phone, email, address, laptop } = contact.value;

  const propsParams = new URLSearchParams({
    fullname,
    phone,
    email,
    address,
    laptop,
  });

  const fileData = new FormData();
  if (filePicked.value) {
    // Use .value here
    fileData.append("receipt", filePicked.value);
  }

  loading.value = true;

  try {
    const response = await fetch(
      `https://testbackend-ya01.onrender.com/api/v1/users/register?${propsParams.toString()}`,
      {
        method: "POST",
        body: fileData,
      }
    );

    if (!response.ok) {
      const errorData = await response.json();
      console.error(errorData);
    } else {
      const data = await response.json();
      console.log("Here");
      console.log(data);
    }

    // clear the input field when after successful submission
    contact.value = {};

    // Clear the filePicked ref after successful submission
    filePicked.value = null;

    toast("Thank your for registering", {
      autoClose: 3000,
      theme: "light",
      type: "success",
      position: "top-center",
      transition: "slide",
    });

    loading.value = false;
  } catch (error) {
    toast(error.response.data.message, {
      autoClose: 3000,
      theme: "light",
      type: "error",
      position: "top-center",
      transition: "slide",
    });
    console.error(error);
    loading.value = false;
  }
};
</script>

<template>
  <section class="wrapper">
    <div class="inner-wrapper">
      <div class="heading">
        <h1 class="title">Register with us</h1>
        <p class="description">Swap your old laptop for a new one</p>
      </div>
      <form @submit="handleSubmit">
        <div class="contact-box">
          <h3 class="contact-box__title">Full name</h3>
          <input
            v-model="contact.fullname"
            type="text"
            class="contact-box__input"
            placeholder="John Doe"
            required
          />
        </div>
        <div class="contact-box">
          <h3 class="contact-box__title">Phone Number</h3>
          <input
            v-model="contact.phone"
            type="text"
            class="contact-box__input"
            placeholder="+234000000000"
            required
          />
        </div>
        <div class="contact-box">
          <h3 class="contact-box__title">Email</h3>
          <input
            v-model="contact.email"
            type="email"
            class="contact-box__input"
            placeholder="mailat@example.com"
            required
          />
        </div>
        <div class="contact-box">
          <h3 class="contact-box__title">Address</h3>
          <input
            v-model="contact.address"
            type="text"
            class="contact-box__input"
            placeholder="38 Crescent Avenue"
            required
          />
        </div>
        <div class="contact-box">
          <h3 class="contact-box__title">Laptop Specification</h3>
          <select v-model="contact.laptop" class="custom-dropdown__selected">
            <option value="HP">HP</option>
            <option value="DELL">DELL</option>
            <option value="ACER">ACER</option>
            <option value="MACBOOK">MACBOOK</option>
          </select>
        </div>
        <div class="contact-box">
          <h3 class="contact-box__title">Upload receipt of old laptop</h3>
          <div class="contact-box__upload">
            <img
              src="../assets/icons/upload.svg"
              class="upload cursor-pointer"
              alt="upload"
            />
            <input
              type="file"
              class="cursor-pointer"
              style="opacity: 0; position: absolute"
              @change="handleFile"
            />
          </div>
          <p class="text-xs">{{ filePicked }}</p>
          <button class="btn" :disabled="loading" type="submit">
            <span v-if="loading" class="spinner"></span>
            <span v-else>Submit</span>
          </button>
        </div>
      </form>
    </div>
  </section>
</template>

<style scoped>
.wrapper {
  display: flex;
  background: #f9f9f9;
  padding: 3rem 1rem;
  flex-basis: 100%;
  overflow: auto;
}

.inner-wrapper {
  display: flex;
  flex-direction: column;
  width: 100%;
  gap: 25px;
}
.heading {
  color: #000;
  line-height: normal;
}
.title {
  font-size: 2rem;
  font-weight: 600;
}

.description {
  font-size: 1rem;
  font-weight: 300;
}

.contact-box {
  display: grid;
  gap: 8px;
  margin-bottom: 1rem;
}

.contact-box__title {
  color: #1f1f1f;
  font-size: 1rem;
  font-weight: 500;
  line-height: 1.5;
}

.contact-box__input {
  width: 100%;
  padding: 12px;
  border: none;
  font-size: 0.8rem;
  font-weight: 400;
  border-radius: 0.5rem;
  background: #fff;
  line-height: 1.5;
  color: black;
}

.contact-box__upload {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 7rem;
  padding: 3px 0;
  border-radius: 0.5rem;
  background: #fff;
  cursor: pointer;
}

.contact-box__input::placeholder {
  color: #b3b3b3 !important;
}

.btn {
  display: flex;
  padding: 20px 48px;
  justify-content: center;
  align-items: center;
  gap: 10px;
  width: 100%;
  color: #fff;
  font-size: 1rem;
  font-weight: 500;
  line-height: 10px;
  border-radius: 8px;
  background: #335ca6;
  margin-top: 1rem;
  transition: background 300ms ease-in;
}

.btn:hover {
  background: #6d2bbd;
  /* transform: scale(0.9); */
}
/* .btn:focus,
.btn:focus-visible {
  outline: 4px auto #bdc0ec;
} */

/* dropdown */
.custom-dropdown {
  position: relative;
  width: 100%;
  font-family: Arial, sans-serif;
}

.custom-dropdown__selected {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px;
  border: none;
  font-size: 0.8rem;
  border-radius: 4px;
  cursor: pointer;
  background-color: #fff;
}

.custom-dropdown__options {
  position: absolute;
  top: 100%;
  left: 0;
  width: 100%;
  max-height: 200px;
  overflow-y: auto;
  list-style: none;
  margin: 0;
  padding: 0;
  border: 1px solid #ccc;
  border-top: none;
  border-radius: 0 0 4px 4px;
  background-color: #fff;
  display: none;
}

.custom-dropdown__options li {
  padding: 10px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.custom-dropdown__options li:hover {
  background-color: #f0f0f0;
}

.spinner {
  border: 4px solid rgba(255, 255, 255, 0.3);
  border-top: 4px solid #335ca6;
  border-radius: 50%;
  width: 20px;
  height: 20px;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

@media (min-width: 600px) {
  .wrapper {
    padding: 50px 50px 40px;
  }
  .inner-wrapper {
    width: 80%;
  }
  .btn {
    width: 50%;
  }
}

@media (min-width: 769px) {
  .wrapper {
    flex-basis: 50%;
    width: 50%;
  }
}

@media (min-width: 1025px) {
  .wrapper {
    flex-basis: 45%;
    width: 45%;
  }
}
</style>
