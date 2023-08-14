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
![DOMNodeClasses](https://github.com/okko222/JS-Document/assets/107776003/a38c91d5-d0e2-4fcd-b20b-11aa84bae48a)

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
