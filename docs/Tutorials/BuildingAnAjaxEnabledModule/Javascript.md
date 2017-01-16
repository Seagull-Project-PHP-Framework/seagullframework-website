---
layout: page
title: Javascript
permalink: /Tutorials/BuildingAnAjaxEnabledModule/Javascript.html
---

<!-- Name: Tutorials/BuildingAnAjaxEnabledModule/Javascript -->
<!-- Version: 1 -->
<!-- Last-Modified: 2007/01/05 18:09:59 -->
<!-- Author: demian -->
<!-- Status: Updated -->

# Example Javascript
	// todo.js
	    function createNewSection(name)
	    {
	        //  get submitted form value
	        var name = $F('sectionName');
	        if (name != '') {
	            //   prepare incremented IDs
	            var nextSectionId = sections.length + 1;
	            var groupId = 'group' + nextSectionId;
	
	            //  build inner divs
	            var innerdiv1 = Builder.node('div', {className:'handle'}, name);
	
	            var innerdiv2 = Builder.node('div', {className:'addtodo'}, [
	                            Builder.node('input', {type:'text', id: 'new_todo_' + nextSectionId}),
	                            Builder.node('input', {type:'button', onclick: 'createNewTodo($(\'new_todo_' + nextSectionId +'\').value, $(\''  +groupId + '\').id);', value:'Add Todo'})
	                            ]);
	
	            var innerdiv3 = Builder.node('div', {style: 'clear:both'});
	
	            //  wrap
	            var innerwrapper = Builder.node('div', {className:'mytitlebar'}, [
	                            innerdiv1,
	                            innerdiv2,
	                            innerdiv3
	                            ]);
	
	            var outerwrapper = Builder.node('div', {id: groupId, className: 'section', style: 'display:none;'});
	            outerwrapper.appendChild(innerwrapper);
	
	            //  append to parent page div
	            sections.push(outerwrapper.id);
	            $('page').appendChild(outerwrapper);
	            Effect.Appear(outerwrapper.id);
	            destroyLineItemSortables();
	            createLineItemSortables();
	            createGroupSortable();
	        }
	    }
	
	    function createNewTodo(str, myid)
	    {
	        if (str != '') {
	            //  build new element and add to DOM
	            var items = document.getElementsByClassName('lineitem');
	            var nextItemId = items.length + 1;
	            var nextItemIdName = 'item_' + nextItemId;
	            var todoSpanIdName = 'todo_item_'+nextItemId;
	            var donespan = Builder.node('span', [Builder.node('a', {href:'#', className:'toggleDone', onclick:'toggleDone(\''+todoSpanIdName+'\')'}, '[X]')]);
	            var todospan = Builder.node('span', {id:todoSpanIdName, className:'todo'}, str);
	            var newDiv = Builder.node('div', {className: 'lineitem', id: nextItemIdName, style: 'position: relative'}, [
	                donespan,
	                todospan]);
	            items.push(newDiv.id);
	            $(myid).appendChild(newDiv);
	            Sortable.create(myid, {tag:'div', dropOnEmpty: true, containment: sections, only:'lineitem'});
	        }
	    }
	
	    var handlerFunc = function(t) {
	        Element.update($('response'), t.responseText)
	        Element.show('response');
	        Effect.Fade('response', {duration: 5.0});
	    }
	
	    function createLineItemSortables()
	    {
	        for (var i = 0; i < sections.length; i++) {
	            Sortable.create(sections[i],{tag:'div', dropOnEmpty:true, containment:sections, only:'lineitem'});
	        }
	    }
	
	    function destroyLineItemSortables()
	    {
	        for (var i = 0; i < sections.length; i++) {
	            Sortable.destroy(sections[i]);
	        }
	    }
	
	    function createGroupSortable()
	    {
	        Sortable.create('page',{tag:'div', only:'section', handle:'handle'});
	    }
	
	    function toggleDone(todoId)
	    {
	        if ($(todoId).style.textDecoration == 'line-through') {
	            $(todoId).style.textDecoration = 'none';
	        } else {
	            $(todoId).style.textDecoration = 'line-through';
	        }
	    }
	
	    /*
	    Debug Functions for checking the group and item order
	    */
	    function getGroupOrder()
	    {
	        var sections = document.getElementsByClassName('section');
	        var alerttext = '';
	        sections.each(function(section) {
	            var sectionID = section.id;
	            var order = Sortable.serialize(sectionID);
	            alerttext += sectionID + ': ' + Sortable.sequence(section) + '\n';
	        });
	        alert(alerttext);
	        return false;
	    }