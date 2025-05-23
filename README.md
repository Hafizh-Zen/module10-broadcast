# Module 10

Hafizh Surya Mustafa Zen 2306256343




##  How to run

1. **Build the project**

   ```sh
   cargo build 
   ```

2. **Start the server**

   ```sh
   cargo run --bin server
   ```

   This will bind to `127.0.0.1:2000` and wait for incoming client connections.

3. **Start three clients**
   In *three* separate terminals, run:

   ```sh
   cargo run --bin client
   ```

   Each client will connect to the server on port 2000.

4. **Chat!**

    * Type a message in *any* client and press Enter.
    * The server logs the raw message it received and then broadcasts it back to *all* connected clients.
    * Each client prints incoming messages prefixed with `From server:`.

---

##  Demo

![Proof of Running](image/broadcast1.png)
![Proof of Running](image/broadcast2.png)

<h2>2.2 Modifying the WebSocket Port</h2>

<h3>1. Where to Modify</h3>

<ul>
  <li><strong>Server</strong> (<code>server/src/main.rs</code>):</li>
</ul>
<pre><code class="language-diff">
- let listener = TcpListener::bind("127.0.0.1:2000").await?;
+ let listener = TcpListener::bind("127.0.0.1:8080").await?;
</code></pre>

<ul>
  <li><strong>Client</strong> (<code>client/src/main.rs</code>):</li>
</ul>
<pre><code class="language-diff">
- let socket = TcpStream::connect("127.0.0.1:2000").await?;
+ let socket = TcpStream::connect("127.0.0.1:8080").await?;
</code></pre>

<p>There are no other port references—this is raw TCP with newline-delimited UTF-8, not HTTP or WebSocket.</p>

<h3>2. Expected Behavior When Mismatched</h3>

<p>If one side still uses port 2000 and the other uses port 8080, clients will immediately see:</p>

<pre><code>
Connection refused (os error 10061)
</code></pre>

<p>because nothing is listening on the target port.</p>

<h3>3. Testing on Port 8080</h3>

<ol>
  <li>Rebuild and start the server:</li>
</ol>
<pre><code class="language-bash">
cargo run --bin server
</code></pre>

<p>You should see:</p>
<pre><code>
listening on port 8080
New connection from 127.0.0.1:xxxxx
From client 127.0.0.1:xxxxx: "hello"
</code></pre>

<ol start="2">
  <li>In separate terminals, run each client:</li>
</ol>
<pre><code class="language-bash">
cargo run --bin client
</code></pre>

<ol start="3">
  <li>Type messages—each client should receive:</li>
</ol>
<pre><code>
From server: &lt;your message&gt;
</code></pre>

<h3>4. Demo (Port 8080)</h3>

<p><img src="image/broadcast3.png" alt="Proof of Running on 8080" /></p>
