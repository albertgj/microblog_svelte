<h2 id="introduction">Introduction</h2>
<hr />
<p>
  This Web App was created with
  <a href="https://svelte.dev">SvelteJS</a>
  and it uses the
  <a href="https://github.com/albertgj/microblog">microblog rest api</a>
  .
</p>
<h2 id="known-problems">Known Problems</h2>
<hr />
<ul>
  <li>
    THERE IS A PROBLEM WITH THE ROUTING LIBRARY USED IN THIS APP
    <ul>
      <li>
        <strong>
          To avoid any problems DO NOT refresh the page when you navigate
          between pages. You can refresh only when you are in the homepage,
          otherwise the router doesn&#39;t find the pages.
        </strong>
      </li>
    </ul>
  </li>
</ul>
<h2 id="instructions">Instructions</h2>
<hr />
<p>First of all pull the microblog rest api, to get the newest features.</p>
<p>Run the microblog rest api and mysql workbench.</p>
<p>
  <strong>
    The registration of a new user gives this user a &quot;USER ROLE&quot; by
    default
  </strong>
</p>
<p>
  <strong>
    Also when you REGISTER a new user you have to navigate to the login page to
    signin.
  </strong>
</p>
<p>
  <strong>
    You can use the two admin users that are saved in the db automatically:
  </strong>
</p>
<table>
  <tr>
    <th>USERNAME</th>
    <th>PASSWORD</th>
  </tr>
  <tr>
    <td>admin1</td>
    <td>root</td>
  </tr>
  <tr>
    <td>admin2</td>
    <td>root</td>
  </tr>
</table>

<h2 id="configuration">Configuration</h2>
<hr />
<p>
  <em>
    Looking for a shareable component template? Go here --&gt;
    <a href="https://github.com/sveltejs/component-template">
      sveltejs/component-template
    </a>
  </em>
</p>
<h2 id="get-started">Get started</h2>
<hr />
<p>Install the dependencies...</p>
<pre>
  <code class="lang-bash">
    <span class="hljs-built_in">cd</span>
    microblog-svelte npm install
  </code>
</pre>
<p>
  ...then start
  <a href="https://rollupjs.org">Rollup</a>
  :
</p>
<pre>
  <code class="lang-bash">
    npm
    <span class="hljs-keyword">run</span>
    <span class="bash">dev</span>
  </code>
</pre>
<p>
  Navigate to
  <a href="http://localhost:5000">localhost:5000</a>
  . You should see your app running. Edit a component file in
  <code>src</code>
  , save it, and reload the page to see your changes.
</p>
<p>
  By default, the server will only respond to requests from localhost. To allow
  connections from other computers, edit the
  <code>sirv</code>
  commands in package.json to include the option
  <code>--host 0.0.0.0</code>
  .
</p>
<h2 id="building-and-running-in-production-mode">
  Building and running in production mode
</h2>
<hr />
<p>To create an optimised version of the app:</p>
<pre>
  <code class="lang-bash">
    npm
    <span class="hljs-keyword">run</span>
    <span class="bash">build</span>
  </code>
</pre>
