<template>
  <section class="section" id="Subject">
    <br>
    <br>
    <br>
    <h1 class="title">Subjects</h1>
    <div v-if="subjects" class="content-wrapper">
      <div class="table-wrapper">
        <table class="subjects-table">
          <thead>
            <tr>
              <th>ชื่อวิชา</th>
              <th>รหัสวิชา</th>
              <th>เครดิต</th>
              <th>เกรด</th>
              <th>แก้ไข</th>
              <th>ลบ</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(subject, index) in subjects" :key="index">
              <td v-if="editingSubjectIndex !== index">{{ subject.subjectName }}</td>
              <td v-else>
                <input type="text" v-model="subject.subjectName" class="form-input" required placeholder="ชื่อวิชา" />
              </td>
              <td v-if="editingSubjectIndex !== index">{{ subject.subjectCode }}</td>
              <td v-else>
                <input type="text" v-model="subject.subjectCode" class="form-input" required placeholder="รหัสวิชา" />
              </td>
              <td v-if="editingSubjectIndex !== index">{{ subject.credits }}</td>
              <td v-else>
                <select v-model="subject.credits" class="form-input" required>
                  <option value="1">1</option>
                  <option value="2">2</option>
                  <option value="3">3</option>
                </select>
              </td>
              <td v-if="editingSubjectIndex !== index">{{ subject.grade }}</td>
              <td v-else>
                <select v-model="subject.grade" class="form-input" required>
                  <option value="A">A</option>
                  <option value="B+">B+</option>
                  <option value="B">B</option>
                  <option value="C+">C+</option>
                  <option value="C">C</option>
                  <option value="D+">D+</option>
                  <option value="D">D</option>
                  <option value="F">F</option>
                </select>
              </td>
              <td>
                <button v-if="editingSubjectIndex !== index" @click="editSubject(index)" class="edit-btn">แก้ไข</button>
                <div v-else class="button-group">
                  <button @click="saveSubject(subject)" class="save-btn">บันทึก</button>
                  <button @click="cancelEdit()" class="cancel-btn">ยกเลิก</button>
                </div>
              </td>
              <td>
                <button v-if="editingSubjectIndex !== index" @click="deleteSubject(subject.id)" class="delete-btn">ลบ</button>
              </td>
            </tr>
            <tr>
              <td colspan="5">
                <button @click="addSubject()" class="add-btn">เพิ่มวิชา</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <div v-else>
      <p>กำลังโหลดข้อมูล...</p>
    </div>
  </section>
</template>

<script>
export default {
  name: "Subj",
  data() {
    return {
      subjects: [],
      originalSubjects: [],
      editingSubjectIndex: null,
    };
  },
  mounted() {
    this.fetchSubjects();
  },
  methods: {
    fetchSubjects() {
      fetch("http://localhost:3000/subjects")
        .then((response) => response.json())
        .then((subjects) => {
          this.subjects = Array.isArray(subjects) ? subjects : [];
          this.originalSubjects = JSON.parse(JSON.stringify(this.subjects));
        })
        .catch((error) => console.error("Error fetching subjects:", error));
    },
    editSubject(index) {
      this.editingSubjectIndex = index;
    },
    saveSubject(subject) {
      if (subject.id) {
        fetch(`http://localhost:3000/subjects/${subject.id}`, {
          method: "PATCH",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify(subject),
        })
          .then((response) => response.json())
          .then(() => {
            this.fetchSubjects();
            this.editingSubjectIndex = null;
            alert("บันทึกข้อมูลสำเร็จ");
          })
          .catch((error) => console.error("Error updating subject:", error));
      } else {
        fetch(`http://localhost:3000/subjects`, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify(subject),
        })
          .then((response) => response.json())
          .then(() => {
            this.fetchSubjects();
            this.editingSubjectIndex = null;
            alert("บันทึกข้อมูลสำเร็จ");
          })
          .catch((error) => console.error("Error adding subject:", error));
      }
    },
    cancelEdit() {
      this.subjects = JSON.parse(JSON.stringify(this.originalSubjects));
      this.editingSubjectIndex = null;
    },
    addSubject() {
      this.subjects.push({
        subjectName: "",
        subjectCode: "",
        credits: "1",
        grade: "A",
      });
      this.editingSubjectIndex = this.subjects.length - 1;
    },
    deleteSubject(id) {
      fetch(`http://localhost:3000/subjects/${id}`, {
        method: "DELETE",
      })
        .then(() => {
          this.fetchSubjects();
          alert("ลบวิชาสำเร็จ");
        })
        .catch((error) => console.error("Error deleting subject:", error));
    },
  },
};
</script>

<style scoped>
/* ... (your existing styles) ... */
.section {
  padding: 2.5rem;
  color: #dfd0b8;
  border-radius: 15px;
  max-width: 1200px;
  margin: 2rem auto;
  transition: transform 0.3s ease;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.section:hover {
  transform: translateY(-5px);
}

.title {
  font-size: 3.5rem;
  margin: 0 auto;
  margin-bottom: 60px;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
  letter-spacing: 1px;
  color: #ffffff;
}

.content-wrapper {
  display: flex;
  justify-content: center;
  width: 100%;
  gap: 2rem;
}

.table-wrapper {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.subjects-table {
  width: 100%;
  max-width: 800px;
  border-collapse: separate;
  border-spacing: 0;
  margin-bottom: 2rem;
  background: rgba(71, 11, 11, 0.15);
  border-radius: 15px;
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.25);
  overflow: hidden;
}

.subjects-table th,
.subjects-table td {
  padding: 1.5rem;
  text-align: center;
  border-bottom: 2px solid #dfd0b8;
  transition: all 0.3s ease;
}

.subjects-table th {
  background: rgba(255, 255, 255, 0.1);
  color: #ffffff;
  font-size: 1.5rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 1px;
  border-bottom: 3px solid #df644d;
}

.subjects-table td {
  color: lightskyblue;
  font-size: 1.3rem;
  font-weight: 400;
  font-style: italic;
}

.subjects-table tr {
  transition: all 0.3s ease;
}

.subjects-table tr:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(1.01);
}

.subjects-table tr:last-child td {
  border-bottom: none;
}

.form-input {
  width: 100%;
  padding: 0.75rem;
  border-radius: 8px;
  border: 2px solid #ccc;
  background: #ffffff;
  font-size: 1rem;
  transition: all 0.3s ease;
  box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.05);
}

.form-input:focus {
  outline: none;
  border-color: #df644d;
  box-shadow: 0 0 8px rgba(223, 100, 77, 0.3);
}

.button-group {
  display: flex;
  justify-content: center;
  gap: 1rem;
  width: 100%;
}

.edit-btn,
.save-btn,
.cancel-btn,
.add-btn,
.delete-btn {
  padding: 0.75rem 1.5rem;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-weight: 500;
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
}

.edit-btn::after,
.save-btn::after,
.cancel-btn::after,
.add-btn::after,
.delete-btn::after {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 0;
  height: 0;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 50%;
  transform: translate(-50%, -50%);
  transition: width 0.6s ease, height 0.6s ease;
}

.edit-btn:hover::after,
.save-btn:hover::after,
.cancel-btn:hover::after,
.add-btn:hover::after,
.delete-btn:hover::after {
  width: 200%;
  height: 200%;
}

.edit-btn {
  background: #df644d;
}

.edit-btn:hover {
  background: #c5533c;
  transform: translateY(-2px);
}

.save-btn,
.add-btn {
  background: #df644d;
}

.save-btn:hover,
.add-btn:hover {
  background: #c5533c;
  transform: translateY(-2px);
}

.cancel-btn {
  background: #888;
}

.cancel-btn:hover {
  background: #777;
  transform: translateY(-2px);
}

.delete-btn {
  background: #ff4444;
}

.delete-btn:hover {
  background: #cc0000;
  transform: translateY(-2px);
}

/* Animation */
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

.subjects-table,
.subject-item {
  animation: fadeIn 0.5s ease-out forwards;
}

/* Responsive Design */
@media (max-width: 900px) {
  .content-wrapper {
    flex-direction: column;
    align-items: center;
  }

  .table-wrapper,
  .form-wrapper {
    width: 100%;
    justify-content: center;
  }

  .subjects-table {
    max-width: 100%;
  }
}

@media (max-width: 600px) {
  .section {
    padding: 1.5rem;
    margin: 1rem;
  }

  .title {
    font-size: 2.5rem;
    margin-bottom: 40px;
  }

  .subjects-table th,
  .subjects-table td {
    font-size: 1rem;
    padding: 0.75rem;
  }

  .subject-item {
    width: 100%;
    padding: 1rem;
  }

  .button-group {
    flex-direction: column;
    gap: 0.75rem;
  }

  .edit-btn {
    width: 100%;
  }
}
</style>