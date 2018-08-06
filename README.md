# Tips and tricks of primefaces library

## File upload

The component does not have the onclick event to clear previous events. With jquery you can catch the event of "choosing" a file and perform various actions.

```xhtml
<h:form id="frmCarga" enctype="multipart/form-data">
	<p:fileUpload id="fudCargaArchivo" required="true"
		invalidFileMessage="Invalid file"
		invalidSizeMessage="Invalid size"
		fileUploadListener="#{bean.upload}"
		skinSimple="true" auto="true" sizeLimit="1048576"
		allowTypes="/(\.|\/)(zip)$/" />
		
	<p:outputLabel id="lblNombreArchivo" style="margin-left:6px;"
		value="#{bean.fileName}" />
		
	<p:commandButton id="btnCargarArchivo"
		value="Upload"
		actionListener="#{bean.process}" />
</h:form>
```

I need to access other components to modify your css(hide, show) or add styles, etc. To do this we use jquery as shown in the code below.

```html
<script>
 		$(document).on("click", ".ui-fileupload input[type=file]", function(event) {
 			$("#frmCarga\\:btnCargarArchivo").addClass("ui-state-disabled");
 			$("#frmCarga\\:lblNombreArchivo").css("display", "none");
 			$("#msgWarning").css("display", "none");
 			$(this).closest('.ui-fileupload').find('.ui-icon-close').trigger('click');
 		});
</script>
```

Source:
- https://stackoverflow.com/questions/16684750/how-to-display-pfileupload-invalidfilemessage-in-pgrowl
- https://stackoverflow.com/questions/31803012/display-pfileupload-validation-errors-inside-pmessages

## On complete scroll

Call *javascript* function to do scroll from **p:commandbutton** when "oncomplete" action is executed.

```xhtml
<p:commandButton id="btnAction"
	actionListener="#{bean.action}"
	oncomplete="scrollPanel('pnlContenido')" />
```

The javascript function **(jquery)** is named after the primefaces panel ```<p:outputPanel id="pnlContenido" />``` as a parameter to which the scroll must be directed.

```xhtml
<script type="text/javascript">
	// jquery, scroll with effect
	function scrollPanel(panelId) {
		var panel = $('#'+panelId);
		$('html, body').animate({
			scrollTop : panel.offset().top
		}, 1000);
	}
</script>
```

It is also possible to do the same with pure javascript but without animation or effects.

```xhtml
<script type="text/javascript">
    // pure javascript without effect
    function ScrollPage(location) {
        window.location.hash=location;
    }
</script>
```

## Ellipsis text

When we need ellipsis text in ```<p:outputLabel>``` component we can use style directly from component or from a css file.

```xhtml
<!-- style in the component -->
<p:column headerText="Category" style="text-overflow: ellipsis; white-space: nowrap;">
    <p:outputLabel value="#{p.category.name}" style="display: inline;"/>
</p:column>

<!-- or using styleClass --> 
<p:column headerText="Description" sortBy="#{user.description}" styleClass="truncate">
    <h:outputText id="desc" value="#{user.description}"/>
    <pe:tooltip for="desc" value="#{user.description}"/>
</p:column>
```
```css
.truncate {
    max-width: 160px;
    width: 160 px\9;
}

.truncate > div {
    width: 160 px\9;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    -o-text-overflow: ellipsis;
    -ms-text-overflow: ellipsis;
    display: block;
    position: absolute;
}
```
## Label responsive

The component ```p:outputLabel``` generate a _span_ html with a line break(_\<br/>_) inside a _div_. The responsive not work and is necessary add the following: ````style:"display: inline-block;````

`````xhtml
<div class="ui-grid ui-grid-responsive">
	<div class="ui-grid-row">
		<div class="ui-grid-col-4">
			<div class="ui-grid-row" style="display: inline-block;">
				<p:outputLabel value="Value" />
				<br />
				<p:outputLabel value="#{bean.value}"/>
			</div>
		</div>
    </div>
</div>
`````

## Hide message from p:messages

Hide a item from p:messages given a condition.

````xhtml
<p:messages id="msg" closable="false" autoUpdate="false" />
````
Add call to hideMessage function in ````oncomplete p:commandButton````
````javascript
function hideMessage() {
    var errorMessage = document.getElementById("msg").getElementsByClassName("ui-messages-error-summary");
    for (var i = (errorMessage.length - 1) ; i >= 0; i--) {
        if (errorMessage[i].innerHTML === 'Condition.') { //give a condition
            errorMessage[i].parentNode.removeChild(errorMessage[i]); // remove
        }
    }
}
````