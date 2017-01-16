---
layout: page
title: Template
permalink: /Tutorials/BuildingAnAjaxEnabledModule/Template.html
---

<!-- Name: Tutorials/BuildingAnAjaxEnabledModule/Template -->
<!-- Version: 1 -->
<!-- Last-Modified: 2007/01/05 18:12:45 -->
<!-- Author: demian -->
<!-- Status: Updated -->

# Example Template
	<h1 class="pageTitle">TODO Manager</h1>
	<span flexy:if="msgGet()">{msgGet()}</span>
	
	<p>Enter your TODO items below.  Often is helps if you group similar tasks, just drag any item to a group box to achieve this.  When
	an item is complete, mark as done by clicking the [X].  If you later decide an item belongs better in another group, drap it to 
	that group.</p>
	
	<div id="response" style="display:none">Todo saved</div>
	
	<div id="page">
	
	    <div id="createNew">
	
	        <h3>Create New Group</h3>
	        <input type="text" id="sectionName" size="25">
	        <input type="button" onClick="
	                                createNewSection();
	                                new Ajax.Request(
	                                    '{makeUrl(#addGroup#,##,#todo#)}', 
	                                    {asynchronous:true, 
	                                     method:'post',
	                                     parameters:'groupName=' + $('sectionName').value,
	                                     onSuccess:handlerFunc}); 
	                                return false" 
	                            value="Create Group">
	    </div>
	
	    <div flexy:foreach="results,k,oGroup" id="group{oGroup.todo_group_id}" class="section">
	        <div class="mytitlebar">
	            <div class="handle">{oGroup.name}</div>
	            <div class="addtodo">
	                <input  type="text" id="new_todo_{oGroup.todo_group_id}" />
	                <input  type="button" 
	                        onClick="
	                            createNewTodo($('new_todo_{oGroup.todo_group_id}').value, $('group{oGroup.todo_group_id}').id);
	                            new Ajax.Request(
	                                '{makeUrl(#addTodo#,##,#todo#)}', 
	                                {asynchronous:true, 
	                                 method:'post',
	                                 parameters:'todo=' + $('new_todo_{oGroup.todo_group_id}').value + '&groupId='+ {oGroup.todo_group_id},
	                                 onSuccess:handlerFunc}); 
	                            return false;" 
	                        value="Add Todo" />     
	            </div>
	            <div style="clear:both"></div>
	        </div>
	        <div flexy:foreach="oGroup.aTodos,k,oTodo" id="item_{oTodo.todo_id}" class="lineitem">
	            <span>
	                <a onclick="toggleDone('todo_item_{oTodo.todo_id}')" class="toggleDone" href="#">[X]</a>
	            </span>
	            <span class="todo" id="todo_item_{oTodo.todo_id}">{oTodo.description}</span>        
	        </div>
	    </div>
	
	</div>
	
	<fieldset>
	    <legend>Debug and Help Information</legend>
	
	    <input type="button" onClick="getGroupOrder()" value="Debug: Show serialized group order">
	</fieldset>
	
	{scriptOpen:h}
	
	//  get all sections (groups)
	elems = document.getElementsByClassName('section');
	sections = new Array;
	elems.each(function(elems) {
	    sections.push(elems.id);
	});     
	
	    // <![CDATA[
	{foreach:results,k,oGroup}
	Sortable.create('group{oGroup.todo_group_id}', {tag:'div', dropOnEmpty: true, containment: sections, only:'lineitem'});
	{end:}  
	Sortable.create('page',{tag:'div',only:'section',handle:'handle'});
	    // ]]>
	
	{scriptClose:h}