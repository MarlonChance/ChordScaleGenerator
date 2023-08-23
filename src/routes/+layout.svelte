<script lang="ts">
    import { page } from '$app/stores';
    let path: string;

    $: path = $page.url.pathname;

</script>

<nav id="myNAV">
    <a class={path === '/' ? 'active' : ''} href="/">Six/Four String</a>
    <a class={path === '/Seven-Five' ? 'active' : ''} href="/Seven-Five">Seven/Five String</a>
    <a class={path === '/Mandolin' ? 'active' : ''} href="/Mandolin">Mandolin</a>
    <a class={path === '/Ukulele' ? 'active' : ''} href="/Ukulele">Ukulele</a>
    <a class={path === '/Banjo' ? 'active' : ''} href="/Banjo">Banjo</a>
</nav>
<h1>Chord & Scale Finder</h1>
<slot></slot>

<style>
  h1 {
        font-size: 45px;
        font-weight: bolder;        
  }
      /* Add a black background color to the top navigation */
  nav {
    background-color: #FDFFFC;
    overflow: hidden;
    max-width: 50%;
  }

  /* Style the links inside the navigation bar */
  nav a {
    float: left;
    color: black;
    text-align: center;
    padding: 4px 6px;
    text-decoration: none;
    font-size: 17px;
  }

  /* Change the color of links on hover */
  nav a:hover {
    background-color: #9DBEBB;
    color: black;
  }
  nav a.active {
  background-color: grey;
  color: white;
}
</style>