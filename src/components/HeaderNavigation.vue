<template>
<header class="site-header">
    <div class="site-identity">
      <img src="../assets/logo.png" class="site-logo">
      <h1><a href="#">GadgetsOneMalayalam</a></h1>
    </div>  
    <nav class="site-navigation">
      <ul class="nav">
        <li><a href="https://www.youtube.com/@GADGETSONEMALAYALAMTECHTIPS/videos" target="_blank">Youtube</a></li> 
      </ul>
    </nav>
  </header>
</template>

<script>
export default {
    name: 'HeaderNavigation',
}
</script>

<style>
  .site-header { 
    border-bottom: 1px solid #ccc;
    padding: .5em 1em;
    display: flex;
    justify-content: space-between;
  }
  
  .site-logo {
    width: 45px;
    vertical-align: middle;
    padding-right: 15px;
    margin-bottom: 10px;
  }

  .site-identity h1 {
    font-size: 1.5em;
    margin: .6em 0;
    display: inline-block;
  }
  
  
  .site-navigation ul, 
  .site-navigation li {
    margin: 0; 
    padding: 0;
  }
  
  .site-navigation li {
    display: inline-block;
    margin: 1.4em 1em 1em 1em;
  }
</style>