# Why need to care about <Script> tag position in html  
---
  
When HTML parser meet script tag, it stops the process for making DOM and give the autority to javascript engine.   
Then, Javascript engine having control authority loads and parse all the javascript codes in script tag to execute it. when it finishs, control autority back to the HTML parser and restart working on DOM  

**Simply, when browser meet the script tag, may occurs kind of blocking, and process for making DOM may be delayed**


#### Place the Script tag at the bottom of the body element

1. It reduce the page load time.  
because HTML elements are not interfered with rendering due to script loading delays.

2. If Javascript manipulates DOM while DOM is not complete, It may occurs error.
