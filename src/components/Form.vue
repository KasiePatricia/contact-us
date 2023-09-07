<script setup>
import { ref } from "vue";
import axios from "axios";
import { toast } from "vue3-toastify";
import "vue3-toastify/dist/index.css";

const selectedFile = ref("");
const contact = ref({
  name: "",
  number: "",
  email: "",
  address: "",
  laptopType: "",
});

// initial loading state
const loading = ref(false);

// function to upload the receipt
const handleFileChange = (event) => {
  const selected = event.target.files[0];
  if (selected) {
    selectedFile.value = selected.name;
    console.log("Selected file:", selectedFile.value);
  }
};

// const handleSubmit = async () => {
//   loading.value = true;

//   try {
//     let config = {
//       method: "post",
//       maxBodyLength: Infinity,
//       url: "https://testbackend-ya01.onrender.com/api/v1/users/register",
//       headers: {
//         ...data.getHeaders(),
//       },
//       data: JSON.stringify({
//         name: contact.value.name,
//         number: contact.value.number,
//         email: contact.value.email,
//         address: contact.value.address,
//         laptopType: contact.value.laptopType,
//         // Add more fields as needed
//       }),
//     };

//     axios
//       .request(config)
//       .then((response) => {
//         console.log(JSON.stringify(response.data));
//       })
//       .catch((error) => {
//         console.log(error);
//       });

//     // Handle the response if needed
//     console.log("API Response:", response.data);
//   } catch (error) {
//     console.error("Error submitting data:", error);
//   } finally {
//     loading.value = false;
//   }
// };

const handleSubmit = async () => {
  loading.value = true;

  try {
    const response = await axios.post(
      "https://testbackend-ya01.onrender.com/api/v1/users/register",
      {
        name: contact.value.name,
        number: contact.value.number,
        email: contact.value.email,
        address: contact.value.address,
        laptopType: contact.value.laptopType,
        // Add more fields as needed
      }
    );

    // toast("Thank your for registering", {
    //   autoClose: 3000,
    //   theme: "light",
    //   type: "success",
    //   position: "top-center",
    //   transition: "slide",
    // });

    // // clear waitlist details
    // this.contact = {};

    console.log("API Response:", response.data);
  } catch (error) {
    toast(error.response.data.message, {
      autoClose: 3000,
      theme: "light",
      type: "error",
      position: "top-center",
      transition: "slide",
    });
    console.error("Error submitting data:", error);
  } finally {
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
      <form>
        <div class="contact-box">
          <h3 class="contact-box__title">Full name</h3>
          <input
            type="text"
            class="contact-box__input"
            placeholder="John Doe"
            v-model="contact.name"
            required
          />
        </div>
        <div class="contact-box">
          <h3 class="contact-box__title">Phone Number</h3>
          <input
            type="text"
            class="contact-box__input"
            placeholder="+234000000000"
            v-model="contact.number"
            required
          />
        </div>
        <div class="contact-box">
          <h3 class="contact-box__title">Email</h3>
          <input
            type="email"
            class="contact-box__input"
            placeholder="mailat@example.com"
            v-model="contact.email"
            required
          />
        </div>
        <div class="contact-box">
          <h3 class="contact-box__title">Address</h3>
          <input
            type="text"
            class="contact-box__input"
            placeholder="38 Crescent Avenue"
            v-model="contact.address"
            required
          />
        </div>
        <div class="contact-box">
          <h3 class="contact-box__title">Laptop Specification</h3>
          <select v-model="contact.selected" class="custom-dropdown__selected">
            <option value="">Select Laptop Type</option>
            <option value="HP">HP</option>
            <option value="DELL">DELL</option>
            <option value="ACER">ACER</option>
            <option value="MACBOOK">MACBOOK</option>
          </select>
        </div>
        <div class="contact-box">
          <h3 class="contact-box__title">Upload receipt of old laptop</h3>
          <div class="contact-box__upload">
            <img src="../assets/icons/upload.svg" class="upload" alt="upload" />
            <input
              type="file"
              style="opacity: 0; position: absolute"
              @change="handleFileChange"
            />
          </div>
          <p>{{ selectedFile }}</p>
        </div>
      </form>
      <button class="btn" @click="handleSubmit" :disabled="loading">
        <span v-if="loading" class="spinner"></span>
        <span v-else>Submit</span>
      </button>
    </div>
  </section>
</template>

<style scoped>
.wrapper {
  display: flex;
  background: #f9f9f9;
  padding: 3rem 1rem 2rem;
  flex-basis: 100%;
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
  padding: 20px 24px;
  border: none;
  font-size: 1rem;
  font-weight: 400;
  border-radius: 0.5rem;
  background: #fff;
  line-height: 1.5;
}

.contact-box__upload {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 8rem;
  padding: 20px 0;
  border-radius: 0.5rem;
  background: #fff;
}

.contact-box__input::placeholder {
  color: #b3b3b3 !important;
}

.btn {
  display: flex;
  padding: 24px 48px;
  justify-content: center;
  align-items: center;
  gap: 10px;
  width: 100%;
  color: #fff;
  font-size: 16px;
  font-weight: 500;
  line-height: 10px;
  border-radius: 8px;
  background: #335ca6;
}

.btn:hover {
  border-color: #646cff;
}
.btn:focus,
.btn:focus-visible {
  outline: 4px auto #bdc0ec;
}

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
  padding: 20px 24px;
  border: none;
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
    padding: 70px 50px 40px;
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
