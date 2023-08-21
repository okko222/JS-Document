There is two methods to assign a handler. 
1-using HTML attributes on<event> with a function.
2-using DOM property :
<pre>
  <code>
<!--     <input id="elem" type="button" value="Click me"  -->
    <script>
      elem.onClick=function(){
        alert("thank you"); 
      }
    </script>
  </code>
</pre>
-We can't assign more than one event handler otherwise it will overwirtes the existing handler.
-To remove a handler assign <code>elem.onclick=null</code>
-Accessing the element with this:the value of this inside a handler is the element.The one which has the handler on it.
<h5>Possible mistakes</h5>
1-We can set an existing function as a handler the function should be assigned as sayThanks not sayThanks()
2-Don't use setAttribute for handlers 
3-DOM-property case matters elem.onclick not elem.OnClicK DOM prpos case-sensitive
<h5>addEventListener</h5>
Why addEventListener? cuz we can't assign multiple handler to one event.If we do the second handler will overwrite the existing one.
the syntax to add a handler 
<code>element.addEventListener(event,handler,[options])</code>
-To remove the handler use removeEventListener
<code>element.removeEventListener(event,handler,[option])</code>
/!\ for some events handlers only work with addEventListener like DOMContentLoaded event 
document.onDOMContentLoaded=function(){}  will never work!
document.addEventListener("DOMContentLoaded",function()) will!
<h5>Event object</h5>
Some of its props: 
<code>event.type</code>:Event type "click"...
<code>event.currentTarget</code>the same as this
<code>evetn.clientX/clientY</code>
window relative coordinates of the cursor,for pointer events.
-The event object also available in HTML handler
<h5>Object handlers:handleEvent</h5>
We can assign not just a function, but an object as an event handler using addEventListener. When an event occurs, its handleEvent method is called.
