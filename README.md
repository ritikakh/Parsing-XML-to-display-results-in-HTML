# Parsing-XML-to-display-results-in-HTML

	
	ï»¿<html> 
	<head> 
	<title>Company List</title> 
	<style> 
	form { 
	text-align: center; 
	} 
	th { 
	font-weight: bold; 
	font-size:x-large; 
	} 
	</style> 
	<script> 
	function generateHTML(xmlDoc) { 
	ELEMENT_NODE = 1; // MS parser doesn't define Node.ELEMENT_NODE 
	root = xmlDoc.DocumentElement; 
	html_text = "<html><head><title>XML Parse Result</title></head>"; 
	html_text += "<table border='2'>"; 
	caption = "Company List"; 
	html_text += "<caption align='center'><h1>" + caption + "</h1></caption><br><br>"; 
	planes = xmlDoc.getElementsByTagName("Row"); 
	html_text += "</tr>"; 
	//planeNodeList = planes.item(0).childNodes 
	// output out the values 
	for (i = 0; i < planes.length; i++) //do for all planes 
	{ 
	planeNodeList = planes.item(i).childNodes; //get properties of a plane 
	//html_text += "<table border=1 width=20><tbody>" + html_texthd +"<tr><td></td>"; //start a new row of the output table 
	for (j = 0; j < planeNodeList.length; j++) { 
	if (planeNodeList.item(j).nodeType == ELEMENT_NODE) { 
	if (planeNodeList.item(j).nodeName == "Logo") {//handle images separately 
	html_text += "<td><img src='" + planeNodeList.item(j).firstChild.nodeValue + "' width='" + 125 + "' height='" + 125 + "'></td>"; 
	} 
	else if (i == 0) { 
	html_text += "<td align= 'center'><b>" + planeNodeList.item(j).firstChild.nodeValue + "</b></td>"; 
	} 
	else if (planeNodeList.item(j).nodeName == "HomePage") { 
	html_text += "<td><a href='" + planeNodeList.item(j).firstChild.nodeValue + "'>link to company</td>"; 
	} 
	else { 
	html_text += "<td>" + planeNodeList.item(j).firstChild.nodeValue + "</td>"; 
	} 
	} 
	} 
	html_text += "</tr>"; 
	} 
	html_text += "</tbody>"; html_text += "</table>"; 
	html_text += "</bo"+"dy>"+"</html>"; 
	} 
	
	function viewXML(what) { 
	var URL = what.URL.value; 
	function loadXML(URL) { 
	if (URL == "") { 
	window.alert("Please enter the URL."); 
	return; 
	} 
	else if (window.XMLHttpRequest) {// code for IE7+, Firefox, Chrome, Opera, Safari 
	xmlhttp = new XMLHttpRequest(); 
	} 
	else {// code for IE6, IE5 
	xmlhttp = new ActiveXObject("Microsoft.XMLHTTP"); 
	} 
	xmlhttp.open("GET", URL, false); 
	try{ 
	xmlhttp.send(); 
	if (xmlhttp.status == 404) { 
	window.alert("XML file not found"); 
	return; 
	} 
	xmlDoc = xmlhttp.responseXML; 
	if (!xmlDoc) { 
	window.alert("File not found"); 
	return; 
	} 
	else if (xmlDoc.documentElement.nodeName == "parsererror") { 
	window.alert("Invalid XML. It cannot be parsed."); 
	return; 
	} 
	else if (xmlDoc.getElementsByTagName('Workbook')[0].children.length == 0) { 
	window.alert("No Workbook found in XML."); 
	return; 
	} 
	else 
	return xmlDoc; 
	} 
	catch (err) 
	{ 
	alert("File not found."); 
	} 
	} 
	xmlDoc = loadXML(URL); 
	if(xmlDoc.getElementsByTagName("Symbol").item(0) == null) 
	{ 
	alert("No Data"); 
	return; 
	} 
	/*if (xmlDoc.documentElement.nodeName == "parseerror") { 
	window.alert("Error in XML file"); 
	return; 
	}*/ 
	if (window.ActiveXObject) //if IE, simply execute script (due to async prop). 
	{ 
	if (xmlDoc.parseError.errorCode != 0) { 
	var myErr = xmlDoc.parseError; 
	generateError(xmlDoc); 
	hWin = window.open("", "Error"); 
	hWin.document.write(html_text); 
	} else { 
	generateHTML(xmlDoc); 
	hWin = window.open("", "Assignment4"); 
	hWin.document.write(html_text); 
	} 
	} else //else if FF, execute script once XML object has loaded 
	{ 
	xmlDoc.onload = generateHTML(xmlDoc); 
	hWin = window.open("", "Assignment4"); 
	hWin.document.write(html_text); 
	} 
	hWin.document.close(); 
	} 
	</script> 
	</head> 
	<body> 
	<form name="myform" id="location"> 
	Enter URL for Company List XML file<br /><br /> 
	<input type="text" name="URL" maxlength="255" size="25" /><br /><br /> 
	<input type="button" name="submit" value="Submit Query" onclick="viewXML(this.form)" /> 
	</form> 
	<noscript> 
	<div style="display: block; font-family: Verdana, Geneva, Arial; font-size: 10px">
	The University of Southern California does not screen or control the content on this website and thus does not guarantee the accuracy, integrity, or quality of such content. All content on this website is provided by and is the sole responsibility of the person from which such content originated, and such content does not necessarily reflect the opinions of the University administration or the Board of Trustees
	</div>
	</body> 
	</html>

