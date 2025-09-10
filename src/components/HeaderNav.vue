<template>
  <header class="logohead">
    <a href="#home" class="logo">Prang Kanyasiri</a>
    <nav class="navbar">
      <a href="#home">Home</a>
      <a href="#about-me">About Me</a>
      <a href="#achievements">Achievements</a>
      <a href="#skills">skills</a>
      <a href="#contact"  >contact</a>
    </nav>
  </header>
</template>

<script>
export default {
  name: 'Header'
}
</script>
<style scoped>
  a.logo {
  text-decoration: none;
  
}

.navbar a,
.navbar a:link,
.navbar a:visited,
.navbar a:hover,
.navbar a:active,
.navbar a:focus,
.navbar a:focus-visible {
  text-decoration: none !important;    
  text-decoration-color: transparent !important;
  border: none !important;              
  outline: none;                       
  box-shadow: none;
  color: #e2e8f0;
  position: relative;
  padding: 0.25rem 0;
  transition: color 0.3s ease;
  font-size: 2rem;
  font-weight: 500;
}


.navbar a::after {
  content: "";
  position: absolute;
  left: 0;
  bottom: -4px;
  width: 0%;
  height: 2px;
  background-color: #38bdf8;
  transition: width 0.3s ease;
}


.navbar a:hover {
  color: #38bdf8;
}
.navbar a:hover::after {
  width: 100%;
}

.navbar a:focus-visible {
  outline: 2px solid #38bdf8;
  outline-offset: 4px;
}




</style>
