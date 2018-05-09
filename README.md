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

