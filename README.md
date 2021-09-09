# Sharepoint-Related-List-New-Item-Autopopulate

This script solves the issue of auto-populating a field on a related list when adding a new item.

## Requirements:

* The parent and child list should both share the same Lookup Field. 

## Instructions:

* Open the child form you would like to auto-populate.
* Click edit page
* Add a Web Part
* Select Media and Content -> Script editor
* Paste the contents of file script.html 
* Replace both instances of THE TITLE OF YOUR FIELD LOOKUP FIELD with the name of your lookup field
* **Make sure the web part is at the bottom of the form.**
 * To move it down -> Click Edit Web Part -> Layout -> Set Zone Index 2 or larger depending on the number of web parts.
* Save

## Example:

I want to auto-populate the "Case Number" in my form:

![Sharepoint Form](https://i.ibb.co/QQDkcw4/Screen-Shot-2021-09-09-at-1-51-38-PM.png)

I add a new Web Part and paste the following code:

```javascript
/* Get the parent list URL */
const listUrl = document.referrer;

/* We will pass the title of our lookup field to this function and it will pass the ID to the Populate Fields function*/
function queryListId(){
  /* Split the parent list url after "&" into separate strings */ 
  const firstSplit = listUrl.split("&");
  /* We know the first string of the new array contains the ID, so we split it again to isolate the ID number */ 
  const secondSplit = firstSplit[0].split("=");
  /* Pass the ID number which is the second string in the secondSplit array */
  return secondSplit[1];
}

/* This function will populate the fields that you target using jQuery */
function populateFields(){
    /* Target a select field by its title, replace its value with the parent lookup field value and disable it to keep consistency*/
  $("select[title='THE TITLE OF YOUR FIELD LOOKUP ']").val(queryListId("THE TITLE OF YOUR FIELD LOOKUP ")).attr('disabled', true);
}

/* Ask Sharepoint to load our functions after loading everything else*/
_spBodyOnLoadFunctionNames.push("populateFields");
```

**I replace "THE TITLE OF YOUR FIELD LOOKUP FIELD" with "Case Number"**

```javascript
  $("select[title='Case Number']").val(queryListId("Case Number")).attr('disabled', true);

```

Save.

I go back to my list, add a new item, and the Case Number field is now populated with its parent.

![Sharepoint Form](https://i.ibb.co/5xB8jyY/Screen-Shot-2021-09-09-at-2-12-15-PM.png)

