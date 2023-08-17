<b>Here’s a bird’s-eye view of what we have when JavaScript runs in a web browser</b>

![window](https://github.com/okko222/JS-Document/assets/107776003/a9dc0540-b09b-4644-8253-29e7506a5cbc)

<ul>
  <li><b>window:</b> it is a global object for JavaScript code
    <pre><code>
        function sayHi() {
        alert("Hello");
      }// global functions are methods of the global object:window.sayHi(); //it shows Hello</code></pre>
    -And we can use it as a browser window, to show the window height: 
    <code>alert(window.innerHeight)</code>
  </li>
  <li><b>DOM:</b> The Document Object Model, or DOM for short, represents all page content as objects that can be modified.
The document object is the main “entry point” to the page. We can change or create anything on the page using it.
</li>
<li><b>BOM:</b> The Browser Object Model (BOM) represents additional objects provided by the browser (host environment) for working with everything except the document.
For instance:
The navigator object provides background information about the browser and the operating system. There are many properties, but the two most widely known are: 
  <ol>
<li>navigator.userAgent – about the current browser, and navigator.platform – about the platform (can help to differentiate between Windows/Linux/Mac etc).</li>
<li>The location object allows us to read the current URL and can redirect the browser to a new one.</li>
  </ol>
</li>
</ul>
-every HTML tag is an object <code>document.body</code> is the object representing the body tag.
/!\ An interesting "special case" is tables.By DOM specification they must have <code>tbody</code> tag, but html text may omit it.Then the browser creates <tbody> in the DOM automatically.
-There are other node types besides elements and text nodes for example comments :
    comments #comment why is a comment added to the DOM?it doesn't affect the visual representation in any way. But there’s a rule – if something’s in HTML, then it also must be in the DOM tree.
-the <!DOCTYPE...> directive at the very beginning of HTML is also a DOM node. It’s in the DOM tree right before <html>. Few people know about that. We are not going to touch that node, we even don’t draw it on diagrams, but it’s there. 
-The document object that represents the whole document is, formally, a DOM node as well.
-There are 12 node types. In practice we usually work with 4 of them:
<ol>
  <li>document - the "entry point" into DOM.</li>
  <li>element nodes - HTML-tags, the tree building blocks</li>
  <li>text nodes - contain text.</li>
  <li>comments - sometimes we can put information there, it won't be shown,but JS can read it from the DOM</li>
</ol>
-Chrome Developer Tools at https://developers.google.com/web/tools/chrome-devtools.
-DOM nodes have <i>properties</i> and <i>methods</i> that allow us to travel between them, modify them, move around the page, and more. 
<h1>Walking the DOM</h1>
-The DOM allows us to do anything with elements and their contents, but first we need to reach the corresponding DOM object.All operations on the DOM start with the document object. That’s the main “entry point” to DOM. From it we can access any node.

  ![DOM](https://github.com/okko222/JS-Document/assets/107776003/30122955-38e8-4a67-aaa4-cdd0a976801f)

The topmost tree nodes are available directly as document properties:
<ol>
  <li> the html = document.documentElement
   The topmost document node is document.documentElement. That’s the DOM node of the <html> tag.</li>
  <li>the  body = document.body</li>
  <li>the head = document.head</li>
</ol>
-In the DOM world null means "doesn't exist" or no such node.
<h4>Children: childNodes, firstChild, lastChild</h4>
There are two terms that we'll use from now : 
<ul>
  <li>Child nodes (or children) – elements that are direct children. In other words, they are nested exactly in the given one. For instance, <head> and <body> are children of <html> element.</li>
    <li>Descendants – all elements that are nested in the given one, including children, their children and so on.</li>
</ul>
<h5>The childNodes collection lists all child nodes,including text nodes.</h5>
index1.html shows children of document.body
<h5>firstChild and lastChild</h5>
elem.childNodes[0] === elem.firstChild
elem.childNodes[elem.childNodes.length-1] === elem.lastChild
-There is also a special function <code>elem.hasChildNodes</code>
to check wether there are any child nodes.
<h5>DOM collections</h5>
-childNodes looks like arrays.But actually it's not an array,but rather a collection a special array-like iterable object means:
<ul>
  <li>We can use for..of to iterate over it</li>
  <li>Array methods won't work, because it's not an array if we want we can use Array.from to create a real array.</li>
</ul>
/!\ DOM collections are read-only we can't replace a child by something 
/!\ Almost all DOM collections are live.
/!\ Don't use for..in loop iterates over all enumerable props..and collections have extra propts rarely used.
<h5>Siblings and the parent</h5>
Siblings are nodes that are children of the same parent.
<ul>
  <li> the body  is said to be the “next” or “right” sibling of head, </li>
  <li>the head is said to be the “previous” or “left” sibling of body.</li>
</ul>
the next sibling is <code>nextSibling</code> and the previous one is <code>previousSibling</code>
<h5>Elements-only navigation</h5>
For instance, in childNodes we can see both text nodes, element nodes, and even comment nodes if they exist.
navigatioin links that only take <i>element nodes</i>
<ul>
  <li>children only those children that are element nodes</li>
  <li>firstElementChild,lastElementChild first and last elements child</li>
  <li>previousElementSibling,nextElementSibling neighbor lements</li>
  <li>parentElement</li>
</ul>
/!\ parentElement and parentNode usually returns the same with the one <b>exception</b> <code>document.documentElement:</code>
<code>alert(document.documentElement.parentNode) //document
alert(document.documentElement.parentElement ); // null
</code> 
because document is not an element node 
<h5>tables</h5>
Certain types of DOM elements may provide additional properties, specific to their type, for convenience like tables.
<ul>
  <li>table.rows the collection of tr elems </li>
  <li>tr.cells collections of td and th </li>
  <li>tr.sectionRowIndex the positioin Index of the given tr inside the enclosing</li>
  <li>tr.rowIndex the number  of the tr in the table as a whole </li>
</ul>
<h1>Searching:getElement*, querySelector*</h1>
<h5>getElementById</h5>
    -getElementById one element so the id must be unique else the behavior of methods that use it is unpredictable.
-getElmentById can be called only on document
<h5>querySelectorAll</h5>
-<code>elem.querySelectorAll(css)</code> returns all elements inside elem matching the given CSS selector
-The call to elem.querySelector(css) returns the first element for the given CSS selector.
<h5>matches</h5>
checks if elem matches the given CSS-selector. It returns true or false. <code>elem.matches(css)</code>
<h5>closest</h5>
-Ancestors of an element are: parent, the parent of parent, its parent and so on
-The method elem.closest(css) looks for the nearest ancestor that matches the CSS-selector. The elem itself is also included in the search.index2.html
<h5>getElementsByTagName(tagName),elem.getElementsByClassName(className),document.getElementsByName(name)</h5>
-return one or collections of the corespoding tags
<h5>Live collections</h5>
-All methods "getElementsBy*" return a live collection.
-In constrast, querySelectorAll returns a static collection.It's like a fixed array of elems.
-elemA.contains(elemB) returns true if elemB is inside elemA or when elemA === elemB.
<h1>DOM node classes</h1>

![DOMNodeClasses](https://github.com/okko222/JS-Document/assets/107776003/8b3bc7e6-0a06-4240-979f-9a75efcc98a1)

<ul>
  The classes are:

<li>EventTarget – is the root “abstract” class for everything.

Objects of that class are never created. It serves as a base, so that all DOM nodes support so-called “events”, we’ll study them later.</li>

<li>Node – is also an “abstract” class, serving as a base for DOM nodes.

It provides the core tree functionality: parentNode, nextSibling, childNodes and so on (they are getters). Objects of Node class are never created. But there are other classes that inherit from it (and so inherit the Node functionality).</li>

<li>Document, for historical reasons often inherited by HTMLDocument (though the latest spec doesn’t dictate it) – is a document as a whole.

The document global object belongs exactly to this class. It serves as an entry point to the DOM.</li>

<li>CharacterData – an “abstract” class, inherited by:

-Text – the class corresponding to a text inside elements, e.g. Hello in <p>Hello</p>.
-Comment – the class for comments. They are not shown, but each comment becomes a member of DOM.
</li>
<li>Element – is the base class for DOM elements.

It provides element-level navigation like nextElementSibling, children and searching methods like getElementsByTagName, querySelector.

A browser supports not only HTML, but also XML and SVG. So the Element class serves as a base for more specific classes: SVGElement, XMLElement (we don’t need them here) and HTMLElement.
</li>
<li>Finally, HTMLElement is the basic class for all HTML elements. We’ll work with it most of the time.

It is inherited by concrete HTML elements:

-HTMLInputElement – the class for <input> elements,
-HTMLBodyElement – the class for <body> elements,
-HTMLAnchorElement – the class for <a> elements,
-…and so on.
</li>
</ul>
<h5>nodeType property</h5>
<ol>
  <li>elem.nodeType == 1 for element nodes</li>
  <li>elem.nodeType == 2 for text nodes</li>
  <li>elem.nodeType == 9 for the document object</li>
</ol>
<h5>nodeName and tagName</h5>
alert(document.body.nodeName); // BODY
alert(document.body.tagName); // BODY
What is the difference between them? 
tagName is only supported by element nodes (as it originates from Element class), while nodeName can say something about other node types.
-if we only deal with elements then we can use both tagName and nodeName.
-The browser has two modes of processing documents: HTML and XML In HTML mode tagName/nodeName is always uppercased: it’s BODY either for <body> or <BoDy>.
In XML mode the case is kept “as is”. Nowadays XML mode is rarely used.
<h5>innerHTML:the content</h5>
-If innerHTML inserts a <script> tag into the document – it becomes a part of HTML, but doesn’t execute. 
-innerHTML+= does a full overwrite
<h5>outerHTML:full HTML of the element</h5> 
  index3.html
<b>Beware: unlike innerHTML, writing to outerHTML does not change the element. Instead, it replaces it in the DOM.</b>
<h5>nodeValue/data:text node content</h5>
-The innerHTML property is only valid for element nodes.Other node types, such as text nodes, have their counterpart: nodeValue and data properties.We'll use data because it's shorter.
<h5>textContent:pure text</h5>
The textContent provides access to the text inside the element: only text, minus all <tags>.
-Whey textContent <ul>
<li>With innerHTML we'll have it inserted as html with all tags</li>
<li>With textContent we'll have it inserted as text,all symbols are treated literally. 
**In most cases, we expect the text from a user, and want to treat it as text. We don’t want unexpected HTML in our site. An assignment to textContent does exactly that.**
<h1>Attributes and properties</h1>
 For element nodes, most standard HTML attributes automatically become properties of DOM objects.For instance, if the tag is <body id="page">, then the DOM object has body.id="page".
 -When they are the same, and when they are different?
 <h5>DOM properties</h5> 
 DOM nodes are regular JavaScript objects. We can alter them.
For instance, let’s create a new property in document.body:
<pre><code>document.body.myData={
name:"Caesar",
title:"imperator"}; 
alert(document.body.myData.title);//Imperator;</code></pre> 
-we can a method as well.
<h5>HTML attributes</h5> 
-In HTML, tags may have attributes. When the browser parses the HTML to create DOM objects for tags, it recognizes standard attributes and creates DOM properties from them.

So when an element has id or another standard attribute, the corresponding property gets created. But that doesn’t happen if the attribute is non-standard.
-How to access Non-standard attributes ? <ul><li><code>elem.hasAttribute(name)</code> checks for existence</li>
<li><code>elem.getAttribute(name)</code> get the value</li>
<li><code>elem.setAttribute(name,value) </code>sets the value</li>
<li>elem.removeAttribute(name) removes the attribute</li>
</ul>
-We can read all attributes with <code>elem.attributes</code>
-HTML attributes have the following features: 
1-Their name is case-insensitive (id is like ID)
2-Their values are always strings
-In elem.attributes we can access attr.name and attr.value
-When a standard attribute changes, the corresponding property is auto-updated, and vice versa excepte input.value synchronizes only from attribute → property, but not back.
-Changing the attribute value updates the property.
-But the property change does not affect the attribute.
<h5>Non-standard attributes,dataset</h5>
Sometimes non-standard attributes are used to pass custom data from HTML to JavaScript, or to “mark” HTML-elements for JavaScript.
-Why we should use non-standard attributes the reasons described in index2.html
-why would using an attribute be preferable to having classes .order-state-new .order-state-pending .order-state-canceled (index2.html) ? 
Because an attribute is more convenient to manage. The state can be changed as easy as:

<pre><code>// a bit simpler than removing old/adding a new class
div.setAttribute('order-state', 'canceled');</code></pre>
-But there may be a possible problem with custom attributes. What if we use a non-standard attribute for our purposes and later the standard introduces it and makes it do something? The HTML language is alive, it grows, and more attributes appear to suit the needs of developers. There may be unexpected effects in such case.
To avoid conflicts, there exist data-* attributes.
<h5>data-*</h5>
<b>All attributes starting with “data-” are reserved for programmers’ use. They are available in the dataset property.</b>
-for instance if an elem has an attribute named "data-about",it's available as elem.dataset.about.
-Multiword attributes like <code>data-order-state</code> become camel-cased: 
<code>dataset.orderState</code>
<h1>Modifying the document</h1>
<h5>Creating an element</h5>
<code>document.createElement(tag) tag it can be "div" "p" and so on. </code>
<code>document.createTextNode(text)</code>
-the three steps to create an element in html page : 
<ol>
  <li>create div element <code>let div = document.createElement("div")</code></li>
  <li>set its class according to the needs <code>div.className="alert"</code></li>
  <li>fill it with the content <code>div.innerHTML="content"</code></li>
</ol>
We’ve created the element. But as of now it’s only in a variable named div, not in the page yet. So we can’t see it.
<h5>Insertion methods</h5>
how to make the div show up?
-we can use append on document.body or on any other element.
insertion methods they specify different places where to append: 
<ul>
<li><code>node.append(...nodes or strings)</code> append nodes or strings at the end of node,
</li>
  <li><code>node.prepend(...nodes or strings)</code>– insert nodes or strings at the beginning of node,</li>
  <li><code>node.before(...nodes or strings)</code> –- insert nodes or strings before node,</li>
<li><code>node.after(...nodes or strings)</code> –- insert nodes or strings after node</li>
<li><code>node.replaceWith(...nodes or strings)</code> –- replaces node with the given nodes or strings.</li>
</ol>
-Strings are inserted in a safe way like elem.textContent doest it.
What if we’d like to insert an HTML string “as html”, with all tags and stuff working, in the same manner as elem.innerHTML does it?
<h5>insertAdjacentHTML/Text/Element</h5>
<code>elem.insertAdjacentHTML(where,html)</code>
where : a string parameter
<ul>
  <li>"beforebegin" – insert html immediately before elem,</li>
  <li>"afterbegin" – insert html into elem, at the beginning,</li>
  <li>"beforeend" – insert html into elem, at the end,</li>
  <li>"afterend" – insert html immediately after elem.</li>
</ul>
html: is an HTML string 
<code>elem.insertAdjacentText(where,text)</code> text is inserted as text instead of HTML 
<code>elem.insertAdjacentElement(where,elem)</code> insert element.
-They exist mainly to make the syntax “uniform”. In practice, only insertAdjacentHTML is used most of the time. Because for elements and text, we have methods append/prepend/before/after – they are shorter to write and can insert nodes/text pieces.
<h5>Node removal</h5>
-In order to remove an element or text we use elem.remove();
  <h5>Cloning nodes:cloneNode</h5>
  <code>elem.cloneNode(true) </code>creates a "deep" clone of the element -attributes and subelements and subelements.
  <code>elem.cloneNode(falase)</code> the clone is without child elements
<h5>DocumentFragement</h5>
-DocumentFragment is a special DOM node that serves as a wrapper to pass around lists of nodes.
<pre><code>
  <ul id="ul"></ul>

<script>
function getListContent() {
  let fragment = new DocumentFragment();

  for(let i=1; i<=3; i++) {
    let li = document.createElement('li');
    li.append(i);
    fragment.append(li);
  }

  return fragment;
}

ul.append(getListContent()); // (*)
</script>
</code></pre>
/!\<b> We'll need DocumentFragement when well cove template element</b>
<h5>A word about "document.write"</h5>
-Is a very ancient method to add something to a web page! 
-comes from times there was no DOM,no standard... it still lives bcz there are scripts using it.
<b>The call to document.write only works while the page is loading</b>
<pre><code><p>After one second the contents of this page will be replaced...</p>
<script>
  // document.write after 1 second
  // that's after the page loaded, so it erases the existing content
  setTimeout(() => document.write('<b>...By this.</b>'), 1000);
</script></code></pre>
-There is an upside also Technically, when document.write is called while the browser is reading (“parsing”) incoming HTML, and it writes something, the browser consumes it just as if it were initially there, in the HTML text.
So it works blazingly fast, because there’s no DOM modification involved. It writes directly into the page text, while the DOM is not yet built.
So if we need to add a lot of text into HTML dynamically, and we’re at page loading phase, and the speed matters, it may help. But in practice these requirements rarely come together. And usually we can see this method in scripts just because they are old.
<h1>Styles and classes</h1>
-We should always prefer CSS classes to style.
-We can use <code>elem.classList</code> it's a special object with methods to add/remove/toggle a single class : 
<pre><code>
  <body class="main page">
  <script>
    // add a class
    document.body.classList.add('article');

    alert(document.body.className); // main page article
  </script>
</body>
</code></pre>
In order to change the full class we use className and for individual classes we use classList.
methods of classList: 
<ul>
  <li>elem.classList.add/remove("class")</li>
  <li>elem.classList.toggle("class") adds the class if it doesn't exist otherwise revomes it.</li>
<li>elem.classList.contains("class") checks returns true/false
</ul>
<code>classList</code> is iterable so we can use for..of 
<h5>Resetting the style property</h5>
-In order to remove style.display instead of delete elem.style.display we should assgin an empty string to it elem.style.displaye=""; 
or we can use this method : <code>document.body.style.removeProperty("background")</code>
<h5>Full rewrite with style.cssText</h5>
Normally, we use style.* to assign individual style properties.We can’t set the full style like div.style="color: red; width: 100px", because div.style is an object, and it’s read-only.
To set the full style as a string, there’s a special property style.cssText:index5.html
<b>such assignment removes all existing styles</b> it does not add, but replaces them.<b>The same can be accomplished by setting an attribute: <code>div.setAttribute('style', 'color: red...').</code></b>
<h5>Computed styles:getComputedStyle</h5>
Modifying a style is easy but hwo to read it?we can't read anything comes from CSS classes using elem.style.<code>getComputedStyle(element,[pseudo])</code> 
The result is an object with styles.
/!\<b>We should always ask for the exact property that we want, like paddingLeft or marginTop or borderTopWidth. Otherwise the correct result is not guaranteed.</b>
<h5>Styles applied to :visited links are hidden!</h5>
Visited links may be colored using :visited CSS pseudoclass.But getComputedStyle does not give access to that color, because otherwise an arbitrary page could find out whether the user visited a link by creating it on the page and checking the styles.
JavaScript may not see the styles applied by :visited. And also, there’s a limitation in CSS that forbids applying geometry-changing styles in :visited. That’s to guarantee that there’s no side way for an evil page to test if a link was visited and hence to break the privacy.

