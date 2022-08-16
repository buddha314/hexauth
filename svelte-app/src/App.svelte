<script>
  import './styles.scss';
  import { Router, Link, Route } from 'svelte-routing';
  import About from './About.svelte';
  import Calculator from './calculator/Calculator.svelte';

  import { HeaderBar, NavBar, PageNotFound, Redirect } from './components';
import { component_subscribe } from 'svelte/internal';

  export let url = '';
</script>

<HeaderBar />
<div class="section columns">
  <Router {url}>
    <NavBar />
    <main class="column">
      <div>
        <Route path="/">
          <Redirect path="/calculator" />
        </Route>
        <Route path="/calculator" component={Calculator}/>
        <Route path="/about" component={About} />
        <Route path="**" component={PageNotFound} />
      </div>
    </main>
  </Router>
</div>
