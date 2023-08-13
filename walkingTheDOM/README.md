<b>Here’s a bird’s-eye view of what we have when JavaScript runs in a web browser</b>
![window](https://github.com/okko222/JS-Document/assets/107776003/a9dc0540-b09b-4644-8253-29e7506a5cbc)
<ul>
  <li><b>window:</b> it is a global object for JavaScript code
    <code>
        function sayHi() {
        alert("Hello");
      }// global functions are methods of the global object:window.sayHi(); //it shows Hello</code>
    -And we can use it as a browser window, to show the window height: 
    <code>alert(window.innerHeight)</code>
  </li>
  <li><b>DOM:</b> The Document Object Model, or DOM for short, represents all page content as objects that can be modified.
The document object is the main “entry point” to the page. We can change or create anything on the page using it.
</li>
<li><b>BOM:</b> The Browser Object Model (BOM) represents additional objects provided by the browser (host environment) for working with everything except the document.
For instance:
The navigator object provides background information about the browser and the operating system. There are many properties, but the two most widely known are: <ol><li>navigator.userAgent – about the current browser, and navigator.platform – about the platform (can help to differentiate between Windows/Linux/Mac etc).<li>
<li>The location object allows us to read the current URL and can redirect the browser to a new one.</li></ol></li>
</ul>
