---
layout: post
title: Simple reactivity
---
Why isn't programming more like spreadsheets? Instead of the global variables used in imperative programming, have "rows" defined in terms of events and other rows!

a todo:
 title = trim((new.text on new.submit) or (edit.text on edit.submit))
 completed = item.completed or (true on allComplete.checked)
 removed = true on item.remove.clicked or true on removeAll.clicked or (true on edit.submit when isEmpty(edit.text))

count x:
 cound but exclude removed

<ul>
{foreach todo}
 <li>
 <form name=item>
 <input type=checkbox name=completed>
 <button name=remove>
 </form>
 <form name=edit>
 <input name=text>
 <input type=submit>
 </form>
 </li>
{/foreach}
</ul>
{if (count(todo))}
<form name=new>
 <input name=text>
 <input type=submit>
</form>
{if (count(todo) when completed)>0 }<button name=removeAll/>{/if}
<form>
<input type=checkbox name=allComplete value={ (count(todo) when !completed)>0 }/>
Todos: {count(todo)} item{"s" if count(todo) != 1}
{/if>}

Ids are automatic
"on" for joins, use ids sensibly
"or" and "on" last one wins?
precedence?
checked as cell or event or both?
default booleans to the unspecified value (false above)?