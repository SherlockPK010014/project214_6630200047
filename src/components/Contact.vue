<template>
  <section class="section" id="contact">
    <div class="container">
      <br>
      <br>
      <h1 class="title">Contact</h1>
      <br>
      <div class="contact-card">
        <img src="@/assets/34267.jpg" alt="Profile Picture" class="profile-pic" />
        <div class="contact-info">
          <h4>Email: <span>Kanyasiri.k@ku.th</span></h4>
          <h4>Facebook: <span>Prang Kanyasiri</span></h4>
          <h4>IG: <span>i3kxnyaz_</span></h4>
          <h4>Github: <span>SherlockPK010014</span></h4>
          <h4>Tel.: <span>0969459415</span></h4>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: 'Contact'
}
</script>

<style scoped>
.section {
  text-align: center;
  padding: 7rem;
}
.title {
  font-size: 4rem;
  margin-bottom: 1rem;
  color: #faf8f8;
}
.contact-card {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  background: #f9f9f9;
  padding: 5%;
  border-radius: 60px;
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
  max-width: 700px;
  margin: 0 auto;
  transition: transform 0.3s ease-in-out;
}
.contact-card:hover {
  transform: scale(1.05);
}
.profile-pic {
  width: 250px;
  height: 250px;
  border-radius: 50%;
  margin-right: 3rem;
  transition: box-shadow 0.3s ease-in-out;
}
.profile-pic:hover {
  box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.3);
}
.contact-info {
  text-align: left;
  margin-left: 2rem;
}
.contact-info h4 {
  font-size: 1.5rem;
  color: #555;
  margin: 1rem 0;
}
.contact-info span {
  font-weight: bold;
  color: #222;
}
</style>
