<template>
  <section class="section" id="about-me">
    <div class="about-me-container">
      <div class="Pic-ABU">
        <div class="card">
          <div class="card-face front">
            <img src="@/assets/34285.jpg" alt="abu-pic-front" />
          </div>
          <div class="card-face back">
            <img src="@/assets/34287.jpg" alt="abu-pic-back" />
          </div>
        </div>
      </div>
      <div class="profile-content">
        <br />
        <h1 class="title">About Me</h1>
        <br />
        <div v-if="profile">
          <div v-if="!isEditing">
            <h4>
              <span class="label">ชื่อ - นามสกุล : </span>
              <span class="value">{{ profile.fullNameTH }}</span>
            </h4>
            <br />
            <h4>
              <span class="label">Name : </span>
              <span class="value">{{ profile.fullNameEN }}</span>
            </h4>
            <br />
            <h4>
              <span class="label">Nickname : </span>
              <span class="value">{{ profile.nickname }}</span>
            </h4>
            <br />
            <h4>
              <span class="label">Student ID : </span>
              <span class="value">{{ profile.studentID }}</span>
            </h4>
            <br />
            <h4>
              <span class="label">Major : </span>
              <span class="value">{{profile.major}}</span>
            </h4>
            <br />
            <h4>
              <span class="label">Graduated from high school : </span>
              <span class="value">{{profile.graduated}}</span>
            </h4>
            <br />
            <button class="edit-btn" @click="startEditing">แก้ไขข้อมูล</button>
          </div>
          <div v-else>
            <form @submit.prevent="saveChanges">
              <div class="form-group">
                <label>ชื่อ - นามสกุล (TH) : </label>
                <input
                  type="text"
                  v-model="profile.fullNameTH"
                  class="form-input"
                  required
                  placeholder="กรุณากรอกชื่อ - นามสกุล (TH)"
                />
              </div>
              <div class="form-group">
                <label>Name (EN) : </label>
                <input
                  type="text"
                  v-model="profile.fullNameEN"
                  class="form-input"
                  required
                  placeholder="กรุณากรอกชื่อ - นามสกุล (EN)"
                />
              </div>
              <div class="form-group">
                <label>Nickname :</label>
                <input
                  type="text"
                  v-model="profile.nickname"
                  class="form-input"
                  required
                  placeholder="กรุณากรอกชื่อเล่น"
                />
              </div>
              <div class="form-group">
                <label>Student ID :</label>
                <input
                  type="text"
                  v-model="profile.studentID"
                  class="form-input"
                  required
                  placeholder="กรุณากรอกรหัสนิสิต"
                />
              </div>
              <div class="form-group">
                <label>Major :</label>
                <input
                  type="text"
                  v-model="profile.major"
                  class="form-input"
                  required
                  placeholder="กรุณากรอกสาขาวิชา"
                />
              </div>
              <div class="form-group">
                <label>Graduated from high school : </label>
                <input
                  type="text"
                  v-model="profile.graduated"
                  class="form-input"
                  required
                  placeholder="กรุณากรอกชื่อโรงเรียนเดิมที่เคยศึกษา"
                />
              </div>
              <br />
              <div class="button-group">
                <button type="submit" class="save-btn">บันทึก</button>
                <button type="button" class="cancel-btn" @click="cancelEditing">
                  ยกเลิก
                </button>
              </div>
            </form>
          </div>
        </div>
        <div v-else>
          <p>กำลังโหลดข้อมูล...</p>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: "AboutMe",
  data() {
    return {
      profile: {
        fullNameTH: "",
        fullNameEN: "",
        nickname: "",
        studentID: "",
        major:"",
        graduated:"",
        isActive: false,
      },
      originalProfile: null,
      isEditing: false,
    };
  },
  mounted() {
    fetch("http://localhost:3000/profile")
      .then((response) => response.json())
      .then((data) => {
        console.log("Fetched Data:", data);
        this.profile = { ...data };
        this.originalProfile = { ...data };
      })
      .catch((error) => console.error("Error fetching profile:", error));
  },
  methods: {
    startEditing() {
      this.isEditing = true;
    },
    saveChanges() {
      if (
        !this.profile.fullNameTH?.trim() ||
        !this.profile.fullNameEN?.trim() ||
        !this.profile.nickname?.trim() ||
        !this.profile.studentID?.trim()
      ) {
        alert("กรุณากรอกข้อมูลให้ครบทุกช่อง");
        return;
      }

      fetch("http://localhost:3000/profile", {
        method: "PUT",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(this.profile),
      })
        .then((response) => response.json())
        .then((updatedProfile) => {
          this.profile = { ...updatedProfile };
          this.originalProfile = { ...updatedProfile };
          this.isEditing = false;
          alert("บันทึกข้อมูลสำเร็จ");
        })
        .catch((error) => {
          console.error("Error updating profile:", error);
          alert("เกิดข้อผิดพลาดในการบันทึกข้อมูล");
        });
    },
    cancelEditing() {
      this.profile = { ...this.originalProfile };
      this.isEditing = false;
    },
  },
};
</script>

<style scoped>
.section {
  padding: 2rem;
  background: #153448;
  color: #fff;
  border-radius: 10px;
  max-width: 800px;
  margin: auto;
}

.about-me-container {
  display: flex;
  align-items: flex-start;
  gap: 2rem; 
}

.Pic-ABU {
  width: 400px;
  height: 500px;
  background: #fff;
  border-radius: 10px;
  overflow: hidden;
  margin-left: -20rem;
  margin-top: 15%;
  perspective: 1000px; 
}

.card {
  position: relative;
  width: 100%;
  height: 100%;
  transform-style: preserve-3d; 
  animation: flip 6s infinite; 
}

.card-face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden; 
}

.card-face.front {
  transform: rotateY(0deg); 
}

.card-face.back {
  transform: rotateY(180deg); 
}

.Pic-ABU img {
  width: 80%;
  height: 80%;
  margin-top: 12%;
  object-fit: cover;
  border-radius: 10px;
  margin-left: 1%;
}

@keyframes flip {
  0% {
    transform: rotateY(0deg); 
  }
  45% {
    transform: rotateY(180deg); 
  }
  50% {
    transform: rotateY(180deg); 
  }
  95% {
    transform: rotateY(0deg);
  }
  100% {
    transform: rotateY(0deg); 
  }
}

.profile-content {
  flex: 1;
  text-align: left;
}

.title {
  font-size: 4.5rem;
  margin-bottom: 1rem;
  color: #fff;
  margin-left: 20%;
  margin-top: 12%;
}

.form-group {
  margin-bottom: 1rem;
  margin-left: 20%;
}

.form-input {
  width: 100%;
  padding: 0.5rem;
  border-radius: 5px;
  border: 1px solid #ccc;
  margin-left: 20%;
}

.button-group {
  display: flex;
  justify-content: space-between;
  margin-left: 20%;
}

.edit-btn,
.save-btn,
.cancel-btn {
  padding: 0.5rem 1rem;
  background: #df644d;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-left: 20%;
}

.cancel-btn {
  background: #888;
}

.label {
  font-size: 2rem;
  color: #fff;
  margin-left: 20%;
}

.value {
  font-size: 2rem;
  color: #fff;
}
</style>